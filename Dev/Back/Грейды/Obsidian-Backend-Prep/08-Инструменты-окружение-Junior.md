---
tags: [junior, tools, docker, git, backend]
---

# Инструменты и окружение — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#Инструменты и окружение]]

---

## Git: ежедневные команды

```bash
git clone <url>
git switch -c feat/notes-api
git add internal/processor/
git commit -m "feat: add notes processor"
git push -u origin feat/notes-api
git pull --rebase origin main
git stash / git stash pop
git log --oneline -10
git diff main...HEAD
```

**Conventional commits:** `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`.

Merge-конфликты: открыть файл → найти `<<<<<<<` → выбрать версию → `git add` → продолжить merge/rebase.

---

## `.gitignore` для Go

```
/bin/
*.exe
*.test
/vendor/
.env
.idea/
```

Не коммитить: `.env`, credentials, бинарники.

---

## Docker: multi-stage build

```dockerfile
# build stage
FROM golang:1.23-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /notes ./cmd/notes

# runtime stage
FROM alpine:3.21
RUN adduser -D appuser
USER appuser
COPY --from=builder /notes /notes
ENTRYPOINT ["/notes"]
```

- `CGO_ENABLED=0` — статический бинарник
- Multi-stage — маленький образ без SDK
- Non-root user — безопасность

`.dockerignore` — не копировать `.git`, `tmp`, тестовые данные.

---

## Конфигурация через ENV

```go
type Config struct {
    HTTPPort string `env:"HTTP_PORT" envDefault:"8080"`
    DBDSN    string `env:"DB_DSN,required"`
}

// caarlos0/env
cfg, err := env.Parse(&Config{})
```

Локально: `.env` через `godotenv` (не в репозитории). В K8s — ConfigMap/Secret → ENV.

---

## Линтеры и проверки

```bash
go fmt ./...
goimports -w .
go vet ./...
golangci-lint run
govulncheck ./...
```

| Команда | Зачем |
|---------|-------|
| `go fmt` | Единый стиль |
| `goimports` | Импорты + fmt |
| `go vet` | Подозрительные конструкции |
| `golangci-lint` | Набор линтеров |
| `govulncheck` | Уязвимости в зависимостях |

CI должен падать на lint errors.

---

## IDE и отладка

- **VS Code / GoLand** + Go extension
- **delve:** `dlv debug ./cmd/notes`, breakpoints, inspect variables
- `go run`, `go test -v`, race detector: `go test -race ./...`

---

## Командная строка (база)

```bash
curl -X POST http://localhost:8080/api/v1/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"hello"}'

grep -r "NoteStorage" internal/
ssh user@dev-host  # базово
```

---

## Сборка

```bash
go build -o bin/notes ./cmd/notes
go run ./cmd/notes
CGO_ENABLED=0 GOOS=linux go build -o notes ./cmd/notes
```

---

## Частые ошибки

| Ошибка | Правильно |
|--------|-----------|
| Секреты в `config.yaml` в git | ENV / Vault |
| `latest` без тега в Docker | Версионные теги |
| Root в Dockerfile | `USER appuser` |
| Пропуск `go mod tidy` после `go get` | Всегда tidy |

---

## Как проверить, что понял

- [ ] Собрать Docker-образ сервиса
- [ ] `golangci-lint run` без ошибок
- [ ] Конфиг из ENV, `.env` в `.gitignore`

**Связанные темы:** [[07-Observability-Junior]] · [[09-Тестирование-SoftSkills-Junior]] · [[10-Capstone-проект]]
