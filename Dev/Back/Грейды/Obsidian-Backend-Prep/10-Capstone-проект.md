---
tags: [junior, capstone, project, backend]
---

# Capstone-проект: Notes Service

← [[00-MOC-Backend-Review]] · Финал программы Junior

---

## Цель

Собрать мини-сервис «заметки с событиями» по архитектуре из [[06-Архитектура-Junior]] — упрощённый аналог production payment-сервиса из матрицы [[Требования к грейдам Backend Go-разработчика]].

---

## Структура репозитория

```
notes-service/
├── cmd/notes/main.go
├── internal/
│   ├── api/http/           # chi handlers
│   ├── processor/          # CreateNote, GetNote, ...
│   ├── domain/             # Note, ошибки, валидация
│   ├── storage/mysql/      # CRUD
│   └── integrations/kafka/ # producer, consumer (или outbox worker)
├── deployments/
│   └── changelogs/         # SQL миграции
├── Dockerfile
├── docker-compose.yml      # app + mysql + kafka (опционально redpanda)
├── .env.example
└── go.mod
```

---

## Функциональные требования

### REST API (`:8080`)

| Метод | Путь | Описание |
|-------|------|----------|
| GET | `/api/v1/notes` | Список заметок |
| GET | `/api/v1/notes/{id}` | Одна заметка |
| POST | `/api/v1/notes` | Создать (`{"title":"..."}`) |
| PUT | `/api/v1/notes/{id}` | Обновить title |
| DELETE | `/api/v1/notes/{id}` | Удалить |

### События

При создании заметки:
1. Транзакция: `INSERT notes` + `INSERT outbox`
2. Outbox worker публикует `note.created` в Kafka
3. Consumer логирует или обновляет read-model (минимум — structured log)

### Observability (`:8081`)

- `GET /health` — liveness
- `GET /ready` — ping MySQL
- `GET /metrics` — Prometheus counter/histogram
- Request ID middleware + zap logs

---

## Нефункциональные требования

- [ ] Слои: API → Processor → Storage (см. [[06-Архитектура-Junior]])
- [ ] DI через конструкторы, без глобальных переменных
- [ ] Prepared statements, `defer tx.Rollback()`
- [ ] Graceful shutdown: `server.Shutdown(ctx)` на SIGTERM
- [ ] Dockerfile multi-stage, `CGO_ENABLED=0`
- [ ] `golangci-lint run` проходит
- [ ] Минимум 3 unit test + 1 httptest
- [ ] `docker compose up` поднимает всё

---

## Этапы сборки (по неделям)

| Этап | Неделя | Результат |
|------|--------|-----------|
| 1 | 3–4 | CRUD in-memory на chi + тесты |
| 2 | 5–6 | MySQL + Storage + Processor |
| 3 | 7–8 | Outbox + Kafka event |
| 4 | 9–10 | zap, metrics, Docker, health |
| 5 | 11–12 | Полировка, README, self-review |

---

## Чеклист готовности к Junior-ревью

Проговори вслух и покажи в коде:

- [ ] Объясняю слоистую архитектуру на своём проекте
- [ ] REST handler с валидацией и тестами
- [ ] SQL через prepared statements, транзакция с rollback
- [ ] Понимаю at-least-once и идемпотентность consumer
- [ ] `context` пробрасывается от HTTP до Storage
- [ ] Structured logging с request ID
- [ ] Health/readiness и `/metrics`
- [ ] Dockerfile, conventional commits
- [ ] Могу объяснить каждый файл в `internal/`

---

## docker-compose (минимум)

```yaml
services:
  mysql:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: notes
    ports: ["3306:3306"]

  redpanda:
    image: redpandadata/redpanda:latest
    command: redpanda start --overprovisioned --smp 1 --memory 512M
    ports: ["9092:9092"]

  app:
    build: .
    env_file: .env
    ports: ["8080:8080", "8081:8081"]
    depends_on: [mysql, redpanda]
```

---

## README: что написать

1. Как запустить локально
2. Примеры `curl` для API
3. Схема архитектуры (можно mermaid из [[06-Архитектура-Junior]])
4. Переменные окружения из `.env.example`

---

## Самооценка после capstone

| Блок | Уверенность 1–5 | Пробелы |
|------|-----------------|---------|
| Go | | |
| HTTP/chi | | |
| MySQL | | |
| Kafka | | |
| Архитектура | | |
| Observability | | |
| Инструменты | | |
| Тесты | | |

Пробелы с оценкой 1–2 — вернись к соответствующей заметке `01-09`.

**Матрица:** [[Требования к грейдам Backend Go-разработчика]] · **Карточки:** [[00-Junior-Карточки-QA]]
