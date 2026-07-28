---
title: "Требования к грейдам Backend Go-разработчика"
tags:
  - backend
  - golang
  - junior
  - middle
  - grades
  - performance-review
aliases:
  - Go Backend Grades
  - Матрица грейдов Go Backend
status: in-progress
---

# Требования к грейдам Backend Go-разработчика

← Подготовка: [[00-MOC-Backend-Review]] · Карточки: [[Flashcards-All-Questions]]

---

## Краткий ответ (для performance review)

> [!abstract] Простыми словами — вся суть
> **Junior** уверенно работает с базовыми конструкциями Go, HTTP/gRPC, MySQL, Kafka, тестами и инструментами команды. Он берёт понятные задачи, задаёт вопросы заранее и доводит реализацию до ревью.
> **Middle** самостоятельно проектирует решения, учитывает конкурентность, транзакции, отказоустойчивость, наблюдаемость, безопасность и эксплуатацию в Kubernetes. Он владеет фичей целиком: от декомпозиции до деплоя и поддержки.

**Как пользоваться вопросами:**

1. Идём по одному вопросу и проговариваем ответ вслух.
2. В ответе называем определения, показываем пример из Go/проекта и типичные ошибки.
3. Для ревью готовим реальные кейсы: что сделал, почему так, какие риски учёл.

---

## 1. [[Junior]]

### Вопрос J1. Go — язык и runtime

- Типы данных: примитивы (`bool`, `string`, числовые типы), `byte`, `rune`
- Составные типы: массивы, слайсы (`append`, `copy`, `len`, `cap`, срезы), мапы (`make`, итерация, проверка ключа)
- Структуры (`struct`): литералы, встраивание (embedding), теги полей
- Указатели: `&`, `*`, передача по значению vs по ссылке, `nil`
- Методы: value receiver vs pointer receiver, когда что использовать
- Интерфейсы: неявная реализация, пустой интерфейс, type assertion, type switch
- Функции: именованные возвращаемые значения, variadic (`...`), функции как значения
- Пакеты и модули: `go mod init`, `go get`, `go mod tidy`, `internal/`, `cmd/`, видимость (exported/unexported)
- Обработка ошибок: `error` как значение, `fmt.Errorf`, `%w` для wrapping, `errors.Is`, `errors.As`
- `defer`: порядок выполнения, типичные случаи (закрытие ресурсов)
- `panic`/`recover`: когда не использовать в бизнес-логике, recovery на границах (HTTP middleware)
- `context.Context`: передача в функции, `context.Background()`, `context.WithCancel()`, `context.WithTimeout()`
- Goroutines: запуск через `go`, понимание конкурентности vs параллелизма
- Channels: буферизованные и небуферизованные, отправка/получение, закрытие канала, `range` по каналу
- Итерация: `for`, `range` по слайсам, мапам, строкам
- Строки и руны: `[]byte` vs `string`, `utf8`, базовая работа с `strings`, `strconv`
- JSON: `encoding/json` — `Marshal`, `Unmarshal`, struct tags (`json:"field_name"`)
- Время: `time` package — `time.Now()`, `time.Parse`, `time.Duration`, таймеры
- Сборка: `go build`, `go run`, `go test`, `CGO_ENABLED=0`, статическая линковка
- Layout проекта: `cmd/<service>/main.go` как точка входа, `internal/` для приватного кода

### Вопрос J2. HTTP REST (chi)

- HTTP: методы (GET, POST, PUT, PATCH, DELETE), коды ответов (2xx, 3xx, 4xx, 5xx)
- `net/http`: `http.Handler`, `http.HandlerFunc`, `http.ResponseWriter`, `*http.Request`
- chi (`go-chi/chi/v5`): `chi.NewRouter()`, маршрутизация, URL-параметры (`chi.URLParam`)
- Middleware chain: порядок выполнения, `r.Use()`, типичные middleware (logging, recovery)
- Request context: `r.Context()`, передача данных через context
- JSON API: `json.NewEncoder(w).Encode()`, `json.NewDecoder(r.Body).Decode()`, `Content-Type: application/json`
- Валидация входных данных на базовом уровне (обязательные поля, типы)
- CORS: зачем нужен, preflight-запросы, базовая настройка
- Rate limiting: понимание зачем, базовое использование middleware
- Health checks: отдельный HTTP-сервер для liveness/readiness probes
- Концепция нескольких HTTP-серверов в одном процессе: Public API, Admin API, Health
- REST: ресурсы, эндпоинты, CRUD-операции, идемпотентность GET/PUT/DELETE
- Базовое понимание HTTPS, TLS

### Вопрос J3. gRPC

- Protobuf: чтение `.proto` файлов, message, service, rpc
- Unary RPC: клиент и сервер, `google.golang.org/grpc`
- Передача `context.Context` в gRPC-вызовах
- Отличие REST vs gRPC: когда HTTP, когда gRPC (внешний API vs межсервисное взаимодействие)
- gRPC-порт (9090) vs HTTP-порт — разделение ответственности
- Базовое понимание protobuf-контрактов из общей библиотеки (`git.indels.tech/Drivee/common`)

### Вопрос J4. Базы данных (MySQL)

- SQL: SELECT, INSERT, UPDATE, DELETE, WHERE, JOIN (INNER, LEFT), ORDER BY, LIMIT
- `database/sql`: `sql.Open`, `db.Query`, `db.QueryRow`, `db.Exec`, `sql.Rows`, `sql.ErrNoRows`
- Prepared statements: `db.Prepare`, плейсхолдеры `?`, защита от SQL injection
- Транзакции на базовом уровне: `db.Begin`, `tx.Commit`, `tx.Rollback`, `defer tx.Rollback()`
- `sql.NullString`, `sql.NullInt64` и другие nullable-типы
- Connection pool: `SetMaxOpenConns`, `SetMaxIdleConns` — понимание зачем
- Миграции Liquibase: чтение changelog в `deployments/changelogs/`, применение миграций
- Индексы: понимание зачем нужны (без глубокой оптимизации)
- MySQL driver: `go-sql-driver/mysql`, DSN-строка подключения
- Паттерн Storage-слоя: только SQL/CRUD, без бизнес-логики

### Вопрос J5. Messaging (Kafka)

- Kafka: topic, partition, offset, broker — базовые понятия
- Producer: отправка сообщений в topic
- Consumer: чтение сообщений, commit offset
- Consumer group: зачем нужна, rebalance — понимание на концептуальном уровне
- At-least-once delivery: дубликаты возможны, идемпотентность обработки
- franz-go (`twmb/franz-go`): базовое использование producer/consumer
- DLQ (Dead Letter Queue): что это, зачем отдельный topic для «битых» сообщений
- Transactional Outbox: понимание паттерна (запись в БД + outbox-таблица → воркер → Kafka)
- apm-queue (`git.indels.tech/Drivee/apm-queue/v2`): понимание что это обёртка над Kafka

### Вопрос J6. Архитектура приложения

- Слоистая структура: API (HTTP/gRPC) → Processor (бизнес-логика) → Domain / Integrations / Storage
- Storage: только SQL/CRUD, без бизнес-правил
- Processor: оркестрация, вызов Storage и Integrations
- Domain: модели, инварианты, доменные типы
- Integrations: внешние провайдеры, Kafka, Vault
- Фоновые компоненты в том же процессе: Kafka consumer/producer, outbox worker, polling workers
- Stateless: отсутствие shared in-memory state между репликами (2–3 пода в K8s)
- Dependency injection через конструкторы (не глобальные переменные)
- Единый бинарник `cmd/payment/main.go` со всеми компонентами

### Вопрос J7. Observability

- Structured logging: zap (`go.uber.org/zap`) — levels (Debug, Info, Warn, Error), поля (`zap.String`, `zap.Int`, `zap.Error`)
- Логирование ошибок: всегда с контекстом (request ID, user ID, operation)
- Sentry (`getsentry/sentry-go`): когда отправлять ошибки (не все, только неожиданные)
- Prometheus (`prometheus/client_golang`): counter, gauge, histogram — базовые метрики
- Endpoint `/metrics` на порту 8081
- Health/readiness probes: `/health`, `/ready` — зачем K8s их опрашивает

### Вопрос J8. Инструменты и окружение

- Git: `init`, `clone`, `add`, `commit`, `push`, `pull`, `branch`, `checkout`/`switch`, `merge`, `log`, `diff`, `stash`
- Разрешение merge-конфликтов
- `.gitignore`: синтаксис и типичные паттерны для Go
- Docker: Dockerfile, multi-stage build, `CGO_ENABLED=0`, Alpine base image
- `.dockerignore`: не копировать лишнее в контекст сборки
- Конфигурация: `caarlos0/env` — парсинг ENV-переменных, `godotenv` для локальной разработки
- golangci-lint: запуск, чтение и исправление ошибок линтера
- govulncheck: проверка уязвимостей в зависимостях
- VS Code / GoLand: настройка, Go extension, delve debugger
- Командная строка: `cd`, `ls`, `mkdir`, `cat`, `grep`, `curl`, `ssh` (базово)
- `go vet`, `go fmt`, `goimports`

### Вопрос J9. Тестирование

- Понимание зачем нужны тесты, виды тестирования (unit, integration)
- Table-driven tests: паттерн `[]struct{ name, input, want }`
- testify: `assert`, `require` — базовые матчеры
- `httptest`: `httptest.NewRecorder()`, тестирование HTTP handlers
- Моки через интерфейсы: ручная реализация mock-структуры
- `testing.T`: `t.Run()`, `t.Parallel()`, `t.Helper()`
- Покрытие: `go test -cover`, понимание что покрытие ≠ качество

### Вопрос J10. Soft skills

- Выполняет чётко поставленные задачи в срок
- Сообщает о блокировках и трудностях до дедлайна
- Принимает и применяет обратную связь из код-ревью
- Задаёт уточняющие вопросы по задачам
- Следует установленным в команде процессам и соглашениям
- Пишет понятные сообщения коммитов (conventional commits)
- Базовая грамотность в деловой переписке
- Оценивает сроки рутинных задач
- Ведёт заметки о проделанной работе

---

## 2. [[Middle]]

*Включает все требования Junior, плюс:*

> [!abstract] Простыми словами — вся суть Middle
> Middle не просто знает отдельные инструменты, а понимает их границы применимости: где нужна транзакция, где асинхронное событие, как пережить ретраи, как наблюдать систему и как безопасно выкатывать изменения.

### Вопрос M1. Go (продвинутый)

- `sync` package: `Mutex`, `RWMutex`, `WaitGroup`, `Once`, `sync.Map`
- Race detector: `go test -race`, понимание data races
- Worker pool: паттерн ограничения параллелизма через channel + WaitGroup
- `context`: cancellation propagation, `context.WithDeadline`, `context.WithValue` (осторожно), `context.Cause`
- `select`: мультиплексирование каналов, `default` case, таймауты через `select`
- Memory model Go: happens-before, видимость записей между goroutines
- `pprof`: CPU profiling, heap profiling, `go tool pprof`, анализ hot paths
- Идиомы: accept interfaces, return structs; small interfaces; composition over inheritance
- Generics (Go 1.18+): type parameters, constraints, когда использовать
- `embed`: встраивание статических файлов в бинарник
- Benchmarks: `testing.B`, `b.ResetTimer()`, сравнение производительности
- Escape analysis: понимание stack vs heap allocation (концептуально)
- Итераторы (Go 1.23+): `range` over functions

### Вопрос M2. HTTP / gRPC (продвинутое)

- Проектирование REST API: версионирование (`/v1/`, `/v2/`), пагинация (offset, cursor), фильтрация, сортировка
- Идемпотентность: `Idempotency-Key` header, безопасные повторы POST
- Middleware: auth (JWT extraction), request ID, logging с duration, recovery с Sentry, rate limiting per client
- Graceful shutdown: `server.Shutdown(ctx)`, обработка SIGTERM/SIGINT, drain connections
- Несколько HTTP-серверов: lifecycle management, общий context для shutdown
- gRPC interceptors: logging, auth, recovery, metrics
- gRPC deadlines и cancellation через context
- gRPC error codes: `status.Error`, `codes.NotFound`, `codes.InvalidArgument`, маппинг в HTTP
- Streaming RPC: server streaming, client streaming — понимание когда применять
- Content negotiation, `Accept` header
- Request validation: структурная валидация, кастомные правила

### Вопрос M3. Базы данных (продвинутое)

- `SELECT FOR UPDATE`: пессимистичная блокировка строк в транзакции
- CAS (Compare-And-Swap): `UPDATE ... WHERE version = ?`, optimistic locking
- `FOR UPDATE SKIP LOCKED`: конкурентные воркеры без блокировки друг друга
- Индексы: составные, covering index, EXPLAIN ANALYZE, когда индекс не используется
- N+1 problem: обнаружение и устранение (JOIN, batch loading)
- Transactional Outbox: реализация outbox-таблицы + воркер с `SKIP LOCKED`
- Liquibase: написание миграций (changeset, rollback), naming conventions
- Connection pool tuning: `SetMaxOpenConns`, `SetConnMaxLifetime`, мониторинг pool exhaustion
- Deadlocks: обнаружение, retry-стратегии, порядок блокировок
- Read replicas: понимание read/write splitting (концептуально)
- Миграции без downtime: expand-contract pattern

### Вопрос M4. Messaging (продвинутое)

- Avro: схема, сериализация/десериализация через `linkedin/goavro/v2`
- Schema Registry (Apicurio): регистрация схем, совместимость (backward/forward)
- Confluent wire format: magic byte + schema ID + payload
- Consumer groups: rebalance strategies, partition assignment, consumer lag
- Exactly-once semantics: transactional producer, idempotent producer
- DLQ routing: автоматическая отправка в DLQ после N retries, replay из DLQ
- apm-queue/v2: Producer, Consumer, конфигурация, обработка ошибок
- Backpressure: ограничение скорости обработки, pause/resume consumer
- Message ordering: гарантии порядка в пределах partition
- Event schema evolution: добавление полей, nullable, defaults

### Вопрос M5. Интеграции и домен

- HashiCorp Vault: инъекция секретов через ENV в K8s, rotation
- Платёжные провайдеры: T-Bank, Sber (SberPay, acquiring), Wallet One, Mozen
- Webhook/callback обработка: верификация подписи, идемпотентность, retry с exponential backoff
- `cmd/sber-signer`: зачем выносить криптографическую подпись в отдельный сервис
- Polling workers: T-Bank GetState worker — проверка зависших платежей
- Flipt feature flags: client-side evaluation, gradual rollout
- OAuth flows: Sber API (привязка карт, authorization code)
- Circuit breaker: защита от каскадных сбоев внешних сервисов
- Timeout и retry policies для HTTP-клиентов

### Вопрос M6. ClickHouse

- Audit-логи: зачем отдельное OLAP-хранилище для логов
- Batch insert: эффективная запись пачками
- Отличие OLTP (MySQL) vs OLAP (ClickHouse): когда что использовать
- Схема audit-таблиц: timestamp, event_type, payload, user_id
- Асинхронная запись: не блокировать основной flow

### Вопрос M7. Observability (продвинутое)

- zap + zapsentry: интеграция логов с Sentry, breadcrumbs
- RED metrics: Rate, Errors, Duration — для каждого endpoint
- USE metrics: Utilization, Saturation, Errors — для ресурсов
- Grafana dashboards: создание и чтение дашбордов
- Alertmanager: правила алертов, severity, routing, silencing
- Distributed tracing: OpenTelemetry (концептуально), trace ID propagation
- SLO/SLI/error budget: определение и мониторинг
- Structured logging best practices: correlation ID, не логировать PII
- Log aggregation: понимание pipeline логов в K8s

### Вопрос M8. Инфраструктура и DevOps

- Dockerfile best practices: multi-stage, non-root user, minimal Alpine 3.21, layer caching
- Kubernetes: Deployment, Service, Ingress, ConfigMap, Secret
- Probes: liveness vs readiness, `initialDelaySeconds`, `periodSeconds`
- Kustomize: base + overlays для окружений (dev, djem, prod)
- Argo Rollouts: canary deployment, blue-green, анализ метрик при rollout
- GitLab CI: stages (build, test, lint, deploy), jobs, artifacts, cache, rules
- Harbor registry: push/pull образов, тегирование
- nginx Ingress: routing, SSL termination, WAF (ModSecurity)
- Resource limits: requests/limits для CPU и memory
- Horizontal Pod Autoscaler (HPA): базовое понимание
- Environment-specific configs: `deployments/kustomize/payment/overlays/*/custom/config.yaml`

### Вопрос M9. Безопасность

- JWT (`golang-jwt/jwt/v5`): структура (header.payload.signature), claims, expiry, validation
- Paseto (`o1egl/paseto`): отличия от JWT, когда использовать
- Token rotation и refresh tokens
- Валидация входных данных: whitelist, sanitization, max lengths
- SQL injection prevention: prepared statements, никогда конкатенация SQL
- Secrets management: никогда в коде/репозитории, только Vault/ENV
- HTTPS/TLS: certificate validation, mTLS для межсервисного взаимодействия
- WAF/ModSecurity: понимание зачем на Ingress
- Rate limiting: защита от abuse, per-IP, per-user
- OWASP Top 10 для API: injection, broken auth, sensitive data exposure

### Вопрос M10. Архитектура

- Проектирование Processor: декомпозиция бизнес-логики, single responsibility
- Domain invariants: инкапсуляция правил в domain-типах
- Dependency inversion: зависимость от интерфейсов, не реализаций
- Transactional boundaries: где начинается/заканчивается транзакция
- Идемпотентные handlers: безопасные повторы на уровне API
- Multi-replica safety: нет in-memory locks, нет local cache без invalidation
- ADR (Architecture Decision Records): документирование решений
- Error handling strategy: typed errors, error wrapping, маппинг в HTTP/gRPC codes
- Feature flags в архитектуре: Flipt для gradual rollout
- Event-driven patterns: когда событие, когда синхронный вызов

### Вопрос M11. Тестирование (продвинутое)

- mockery v3: генерация моков из интерфейсов (`.mockery.yaml`)
- Integration tests: `go test -tags=integration`, testcontainers для MySQL
- gofakeit: генерация тестовых данных
- Contract tests для Kafka: проверка schema compatibility
- CI pipeline для тестов: параллельный запуск, coverage gates
- Table-driven tests с subtests: `t.Run(tc.name, func(t *testing.T) { ... })`
- Test fixtures: setup/teardown для integration tests
- Flaky tests: диагностика и устранение
- Benchmarks в CI: regression detection

### Вопрос M12. Soft skills

- Самостоятельно декомпозирует задачи и выбирает решения
- Проводит качественные код-ревью с конструктивной обратной связью
- Менторит джуниоров: объясняет решения, проводит pair programming
- Управляет своими приоритетами, предлагает корректировки скоупа задач
- Оценивает сроки своих задач и задач джуниоров с точностью ±20%
- Аргументирует технические решения с бизнес-обоснованием
- Участвует в найме: проводит секции технического собеседования
- Чётко коммуницирует статус задач, риски и блокеры
- Пишет техническую документацию: README, ADR, API-документация
- Понимает продуктовый контекст: зачем делается фича, кто пользователь
- Предлагает улучшения в процессах команды
- Владеет целыми фичами от проектирования до деплоя
