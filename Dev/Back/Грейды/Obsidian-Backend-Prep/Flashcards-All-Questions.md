---
title: "Карточки — все вопросы Backend Go (Junior + Middle)"
tags:
  - backend
  - golang
  - junior
  - middle
  - flashcards
  - performance-review
aliases:
  - Backend Flashcards
  - Карточки Backend Go
---

# Карточки Q/A — Backend Go (22 вопроса)

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика]]

**Как пользоваться:** закрой ответ, проговори вслух, сверься. Для Junior есть развёрнутые заметки `01–09` — ссылки в ответах.

**Формат:** `**Q:**` / `**A:**`. Совместимо с ручным повторением и плагином Obsidian Spaced Repetition (`Question::Answer` при необходимости).

**Всего карточек:** 158 (минимум 1 на каждый из 22 вопросов + дополнительные по ключевым темам).

---

## Junior (J1–J10)

### Вопрос J1 — Go: язык и runtime

**Q (J1):** Чем Go обрабатывает ошибки вместо try/catch?  
**A:** `error` как возвращаемое значение; проверка `if err != nil`, wrapping через `fmt.Errorf("...: %w", err)`.  
Подробнее: [[01-Go-язык-Junior]]

**Q (J1):** Зачем `%w` в `fmt.Errorf`?  
**A:** Оборачивает ошибку, сохраняя цепочку для `errors.Is` и `errors.As`.

**Q (J1):** Чем слайс отличается от массива?  
**A:** Массив фиксированной длины; слайс — дескриптор (указатель, len, cap) над массивом, динамический через `append`.

**Q (J1):** Как безопасно проверить ключ в мапе?  
**A:** `v, ok := m[key]` — `ok == false` значит ключа нет.

**Q (J1):** Что такое embedding в struct?  
**A:** Встраивание поля без имени — методы и поля вложенного типа «поднимаются» на внешний struct (composition вместо наследования).

**Q (J1):** Value receiver vs pointer receiver — когда что?  
**A:** Pointer — если метод меняет struct или struct большой; value — для маленьких immutable типов.

**Q (J1):** Как в Go реализуется полиморфизм?  
**A:** Через интерфейсы с неявной реализацией — тип подходит, если имеет все методы интерфейса.

**Q (J1):** Type assertion и type switch — зачем?  
**A:** Извлечь конкретный тип из `interface{}`/`any`: `v, ok := x.(T)` или `switch v := x.(type)`.

**Q (J1):** Зачем `defer`?  
**A:** Отложить cleanup (закрытие файла, `tx.Rollback`) до выхода из функции; выполняется LIFO.

**Q (J1):** Когда использовать `panic`?  
**A:** Только для невосстановимых ситуаций на границах (middleware recovery), не в бизнес-логике.

**Q (J1):** Зачем `context.Context`?  
**A:** Отмена, таймауты, передача request-scoped данных; всегда первый аргумент после receiver.

**Q (J1):** Чем конкурентность отличается от параллелизма?  
**A:** Конкурентность — структура (goroutines); параллелизм — реальное одновременное выполнение на разных ядрах.

**Q (J1):** Буферизованный vs небуферизованный канал?  
**A:** Небуферизованный блокирует до пары send/receive; буферизованный принимает до `cap` без блокировки получателя.

**Q (J1):** Что такое `internal/` в layout проекта?  
**A:** Пакеты, которые Go запрещает импортировать извне модуля — приватный код сервиса.

**Q (J1):** Exported vs unexported?  
**A:** Имя с большой буквы — экспортируется из пакета; с маленькой — только внутри пакета.

**Q (J1):** Зачем `CGO_ENABLED=0` при сборке?  
**A:** Отключает CGO → статическая линковка, проще переносимый бинарник для Docker/Alpine.

**Q (J1):** Стандартный layout Go-сервиса?  
**A:** `cmd/<service>/main.go` — точка входа; `internal/` — приватный код; опционально `pkg/` для публичных библиотек.

---

### Вопрос J2 — HTTP REST (chi)

**Q (J2):** Какие HTTP-методы для CRUD?  
**A:** GET — читать, POST — создать, PUT — полная замена, PATCH — частичное обновление, DELETE — удалить.  
Подробнее: [[02-HTTP-REST-chi-Junior]]

**Q (J2):** Что означают классы кодов 2xx, 4xx, 5xx?  
**A:** 2xx — успех; 4xx — ошибка клиента; 5xx — ошибка сервера.

**Q (J2):** Что такое `http.Handler`?  
**A:** Интерфейс с методом `ServeHTTP(w ResponseWriter, r *Request)` — любой обработчик запроса.

**Q (J2):** Зачем middleware в chi?  
**A:** Цепочка обработчиков: logging, recovery, auth — выполняются до/после handler в порядке `r.Use()`.

**Q (J2):** Как получить URL-параметр в chi?  
**A:** `chi.URLParam(r, "id")` после маршрута вида `/notes/{id}`.

**Q (J2):** Почему GET и DELETE идемпотентны?  
**A:** Повторный запрос не меняет результат (GET не должен менять состояние; DELETE удаляет один раз).

**Q (J2):** Зачем health/readiness endpoints?  
**A:** K8s опрашивает liveness (жив ли процесс) и readiness (готов ли принимать трафик, например DB доступна).

**Q (J2):** Что такое CORS preflight?  
**A:** Браузер шлёт OPTIONS перед «непростым» cross-origin запросом, чтобы проверить разрешения сервера.

**Q (J2):** Зачем несколько HTTP-серверов в одном процессе?  
**A:** Разделение Public API, Admin API и Health/metrics — разные порты, разные политики доступа и нагрузки.

**Q (J2):** Как передать request ID через слои?  
**A:** Через `r.Context()` — middleware кладёт ID в context, handler и downstream-функции читают его.

---

### Вопрос J3 — gRPC

**Q (J3):** Что описывает `.proto` файл?  
**A:** `message` (структуры данных), `service` и `rpc` (методы) — контракт между клиентом и сервером.  
Подробнее: [[03-gRPC-Junior]]

**Q (J3):** Что такое unary RPC?  
**A:** Один запрос — один ответ; базовый тип gRPC-вызова через `google.golang.org/grpc`.

**Q (J3):** Когда REST, когда gRPC?  
**A:** REST — внешний API, браузеры, простая интеграция; gRPC — межсервисное взаимодействие, строгий контракт, производительность.

**Q (J3):** Зачем отдельный gRPC-порт (например 9090)?  
**A:** Разделение ответственности: HTTP для внешних клиентов, gRPC для внутренних сервисов; разные middleware и политики.

**Q (J3):** Зачем передавать `context.Context` в gRPC?  
**A:** Для deadlines, cancellation и request-scoped метаданных (trace ID, auth).

---

### Вопрос J4 — Базы данных (MySQL)

**Q (J4):** Чем `QueryRow` отличается от `Query`?  
**A:** `QueryRow` — одна строка (`Scan`); `Query` — много строк (`sql.Rows`, итерация).  
Подробнее: [[04-MySQL-database-sql-Junior]]

**Q (J4):** Зачем prepared statements?  
**A:** Плейсхолдеры `?` отделяют SQL от данных → защита от SQL injection, переиспользование плана.

**Q (J4):** Базовый паттерн транзакции в Go?  
**A:** `tx, _ := db.Begin()` → работа → `tx.Commit()` или `defer tx.Rollback()` при ошибке.

**Q (J4):** Зачем `sql.NullString`, `sql.NullInt64`?  
**A:** Явно представляют nullable-колонки БД — отличают «NULL» от zero value Go.

**Q (J4):** Зачем настраивать connection pool?  
**A:** `SetMaxOpenConns` / `SetMaxIdleConns` ограничивают соединения — без лимита можно исчерпать БД или память.

**Q (J4):** Что делает Storage-слой?  
**A:** Только SQL/CRUD, без бизнес-правил — изоляция персистентности от Processor.

**Q (J4):** Зачем индексы в MySQL?  
**A:** Ускоряют поиск по условию WHERE/JOIN; без индекса — full table scan.

**Q (J4):** Что такое Liquibase changelog?  
**A:** Версионированные миграции схемы БД в `deployments/changelogs/` — воспроизводимые изменения структуры.

---

### Вопрос J5 — Messaging (Kafka)

**Q (J5):** Базовые сущности Kafka?  
**A:** Topic, partition, offset, broker — topic разбит на partitions для параллелизма и порядка внутри partition.  
Подробнее: [[05-Kafka-Messaging-Junior]]

**Q (J5):** Зачем consumer group?  
**A:** Несколько consumer'ов делят partitions — масштабирование чтения; rebalance при добавлении/удалении участников.

**Q (J5):** Что такое at-least-once delivery?  
**A:** Сообщение доставлено минимум раз → возможны дубликаты; обработчик должен быть идемпотентным.

**Q (J5):** Что такое DLQ?  
**A:** Dead Letter Queue — отдельный topic для «битых» сообщений после N неудачных попыток обработки.

**Q (J5):** Паттерн Transactional Outbox?  
**A:** Запись в БД + outbox-таблица в одной транзакции → воркер читает outbox → публикует в Kafka → гарантия согласованности.

**Q (J5):** Зачем apm-queue?  
**A:** Обёртка над Kafka (`git.indels.tech/Drivee/apm-queue/v2`) — единый API producer/consumer для сервисов команды.

---

### Вопрос J6 — Архитектура приложения

**Q (J6):** Слоистая структура сервиса?  
**A:** API (HTTP/gRPC) → Processor (бизнес-логика) → Domain / Integrations / Storage.  
Подробнее: [[06-Архитектура-Junior]]

**Q (J6):** Что делает Processor?  
**A:** Оркестрация: вызывает Storage, Integrations, применяет бизнес-правила.

**Q (J6):** Что такое stateless-сервис?  
**A:** Нет shared in-memory state между репликами — любой pod обрабатывает любой запрос; состояние в БД/Kafka.

**Q (J6):** Зачем DI через конструкторы?  
**A:** Явные зависимости, тестируемость (подмена моков), без глобальных переменных.

**Q (J6):** Какие фоновые компоненты в одном бинарнике?  
**A:** Kafka consumer/producer, outbox worker, polling workers — всё в `cmd/payment/main.go`.

**Q (J6):** Что хранит Domain-слой?  
**A:** Модели, инварианты, доменные типы — правила предметной области без инфраструктуры.

---

### Вопрос J7 — Observability

**Q (J7):** Зачем structured logging (zap)?  
**A:** Логи в JSON с полями (`zap.String`, `zap.Int`) — машиночитаемые, фильтруемые, коррелируемые.  
Подробнее: [[07-Observability-Junior]]

**Q (J7):** Какие уровни логов zap?  
**A:** Debug, Info, Warn, Error — выбор уровня по окружению и серьёзности события.

**Q (J7):** Когда отправлять ошибку в Sentry?  
**A:** Неожиданные/необработанные ошибки, не все `err != nil` — иначе шум и alert fatigue.

**Q (J7):** Три типа метрик Prometheus?  
**A:** Counter (монотонный счётчик), Gauge (текущее значение), Histogram (распределение, latency).

**Q (J7):** Зачем `/metrics` на отдельном порту?  
**A:** Изоляция от публичного API; Prometheus scrape без влияния на пользовательский трафик.

---

### Вопрос J8 — Инструменты и окружение

**Q (J8):** Зачем multi-stage Docker build?  
**A:** Стадия сборки (Go toolchain) + минимальный runtime (Alpine) → меньший и безопаснее образ.  
Подробнее: [[08-Инструменты-окружение-Junior]]

**Q (J8):** Зачем `.dockerignore`?  
**A:** Не копировать лишнее в контекст сборки — быстрее build, меньше размер.

**Q (J8):** `caarlos0/env` vs `godotenv`?  
**A:** `env` — парсинг ENV в struct для prod/K8s; `godotenv` — загрузка `.env` для локальной разработки.

**Q (J8):** Зачем golangci-lint?  
**A:** Агрегатор линтеров — единый прогон стиля, ошибок, security-правил перед merge.

**Q (J8):** Зачем govulncheck?  
**A:** Проверка известных уязвимостей в зависимостях Go-модуля.

**Q (J8):** `go vet` vs `go fmt` vs `goimports`?  
**A:** `vet` — подозрительные конструкции; `fmt` — форматирование; `goimports` — fmt + управление import'ами.

---

### Вопрос J9 — Тестирование

**Q (J9):** Чем unit-тест отличается от integration?  
**A:** Unit — изолированная функция/пакет с моками; integration — реальные зависимости (БД, HTTP).  
Подробнее: [[09-Тестирование-SoftSkills-Junior]]

**Q (J9):** Паттерн table-driven tests?  
**A:** `[]struct{ name, input, want }` + цикл `t.Run(tc.name, ...)` — много кейсов в одном тесте.

**Q (J9):** `assert` vs `require` в testify?  
**A:** `require` — останавливает тест при fail; `assert` — продолжает, полезно для нескольких проверок.

**Q (J9):** Как тестировать HTTP handler?  
**A:** `httptest.NewRecorder()` + `handler.ServeHTTP(rec, req)` — без реального сетевого порта.

**Q (J9):** Зачем моки через интерфейсы?  
**A:** Подмена Storage/Integration в тестах Processor без реальной БД или внешних API.

**Q (J9):** Почему покрытие ≠ качество тестов?  
**A:** Можно покрыть 100% тривиальными assert'ами, не проверяя граничные случаи и ошибки.

---

### Вопрос J10 — Soft skills (Junior)

**Q (J10):** Что делать при блокировке до дедлайна?  
**A:** Сообщить заранее: что блокирует, что пробовал, какой нужен help — не молчать до дедлайна.

**Q (J10):** Как принимать код-ревью?  
**A:** Читать комментарии, задавать уточняющие вопросы, применять feedback, не воспринимать как личную критику.

**Q (J10):** Зачем conventional commits?  
**A:** Единый формат (`feat:`, `fix:`, `chore:`) — читаемая история, автогенерация changelog.

**Q (J10):** Когда задавать уточняющие вопросы по задаче?  
**A:** До начала реализации: scope, edge cases, критерии готовности, дедлайн.

**Q (J10):** Зачем вести заметки о работе?  
**A:** Фиксировать решения, блокеры, прогресс — для ревью, онбординга и восстановления контекста.

---

## Middle (M1–M12)

### Вопрос M1 — Go (продвинутый)

**Q (M1):** Когда `sync.Mutex` vs `RWMutex`?  
**A:** Mutex — эксклюзивный доступ; RWMutex — много читателей или один писатель (read-heavy workloads).

**Q (M1):** Зачем `sync.WaitGroup`?  
**A:** Ждать завершения группы goroutines: `Add` → `go` → `Done` → `Wait`.

**Q (M1):** Как обнаружить data race?  
**A:** `go test -race` — race detector находит несинхронизированный доступ к памяти.

**Q (M1):** Паттерн worker pool?  
**A:** Ограниченное число goroutines + channel задач + WaitGroup — контроль параллелизма.

**Q (M1):** Зачем `select` с `default`?  
**A:** Неблокирующее чтение/запись канала или таймаут через `select` + `time.After`.

**Q (M1):** Что такое happens-before в Go?  
**A:** Гарантия видимости записей между goroutines через sync-примитивы, channel ops, `sync.Once`.

**Q (M1):** Зачем `pprof`?  
**A:** CPU/heap profiling — найти hot paths, утечки памяти через `go tool pprof`.

**Q (M1):** Идиома «accept interfaces, return structs»?  
**A:** Функции принимают интерфейсы (гибкость, тесты), возвращают конкретные типы (ясный API).

**Q (M1):** Когда generics в Go?  
**A:** Обобщённые контейнеры/утилиты без `interface{}` и type assertion — не везде, где есть дублирование типов.

**Q (M1):** Зачем `embed`?  
**A:** Встроить статические файлы (шаблоны, миграции) в бинарник без внешних assets.

**Q (M1):** Escape analysis — зачем знать?  
**A:** Понимать, аллоцируется ли на stack или heap — влияет на GC pressure и производительность.

---

### Вопрос M2 — HTTP / gRPC (продвинутое)

**Q (M2):** Способы пагинации REST API?  
**A:** Offset (`?page=2&limit=20`) — просто, но нестабильно при вставках; cursor — стабильнее для больших данных.

**Q (M2):** Зачем `Idempotency-Key` для POST?  
**A:** Безопасные повторы при сетевых сбоях — сервер возвращает тот же результат для того же ключа.

**Q (M2):** Graceful shutdown HTTP-сервера?  
**A:** `server.Shutdown(ctx)` на SIGTERM/SIGINT — перестать принимать новые, дождаться текущих, потом выход.

**Q (M2):** Зачем gRPC interceptors?  
**A:** Cross-cutting concerns: logging, auth, recovery, metrics — как middleware, но для gRPC.

**Q (M2):** Как маппить gRPC codes в HTTP?  
**A:** `codes.NotFound` → 404, `codes.InvalidArgument` → 400, `codes.Internal` → 500 через `status.Error`.

**Q (M2):** Когда streaming RPC?  
**A:** Server streaming — поток ответов (логи, файлы); client streaming — поток запросов (upload); bidirectional — чат.

**Q (M2):** Зачем request validation на уровне API?  
**A:** Структурная валидация + кастомные правила до Processor — ранний fail, понятные 400-ответы.

---

### Вопрос M3 — Базы данных (продвинутое)

**Q (M3):** Зачем `SELECT FOR UPDATE`?  
**A:** Пессимистичная блокировка строк в транзакции — конкурентные обновления не перезапишут друг друга.

**Q (M3):** Optimistic locking через CAS?  
**A:** `UPDATE ... SET version = version + 1 WHERE id = ? AND version = ?` — конфликт, если version изменился.

**Q (M3):** Зачем `FOR UPDATE SKIP LOCKED`?  
**A:** Воркеры берут разные строки без ожидания — очередь задач, outbox worker, polling.

**Q (M3):** Что такое N+1 problem?  
**A:** N запросов вместо одного при загрузке связанных сущностей — лечится JOIN или batch loading.

**Q (M3):** Как реализовать Transactional Outbox?  
**A:** Таблица outbox в той же TX, что бизнес-запись; воркер с `SKIP LOCKED` публикует и помечает обработанным.

**Q (M3):** Как бороться с deadlock?  
**A:** Единый порядок блокировок, короткие транзакции, retry с backoff при `1213 Deadlock`.

**Q (M3):** Паттерн expand-contract для миграций?  
**A:** Expand (добавить новое, писать в оба) → migrate → contract (убрать старое) — без downtime.

**Q (M3):** Зачем `SetConnMaxLifetime`?  
**A:** Ротация соединений — избежать stale connections за LB/proxy, баланс pool exhaustion.

---

### Вопрос M4 — Messaging (продвинутое)

**Q (M4):** Зачем Avro + Schema Registry?  
**A:** Бинарная сериализация со схемой; registry хранит версии, проверяет backward/forward compatibility.

**Q (M4):** Confluent wire format?  
**A:** Magic byte (0) + schema ID (4 bytes) + Avro payload — consumer знает, какую схему применить.

**Q (M4):** Что такое consumer lag?  
**A:** Отставание consumer group от последнего offset — индикатор перегрузки или медленной обработки.

**Q (M4):** Exactly-once в Kafka — как?  
**A:** Idempotent producer + transactional producer (write + offset commit в одной транзакции) — сложно, не всегда нужно.

**Q (M4):** DLQ routing и replay?  
**A:** После N retries → DLQ topic; replay — повторная обработка после фикса бага или данных.

**Q (M4):** Backpressure в consumer?  
**A:** Pause/resume consumption при перегрузке downstream — не накапливать необработанные сообщения.

**Q (M4):** Гарантии порядка сообщений?  
**A:** Порядок сохраняется в пределах одной partition; ключ партиционирования определяет partition.

**Q (M4):** Event schema evolution — правила?  
**A:** Добавлять поля как optional/nullable с defaults; не удалять/менять тип без compatibility check.

---

### Вопрос M5 — Интеграции и домен

**Q (M5):** Зачем HashiCorp Vault в K8s?  
**A:** Секреты не в коде/репо — инъекция через ENV/sidecar, rotation без передеплоя кода.

**Q (M5):** Обработка webhook/callback — три обязательных шага?  
**A:** Верификация подписи, идемпотентность (дедуп по event ID), retry с exponential backoff.

**Q (M5):** Зачем `cmd/sber-signer` отдельно?  
**A:** Криптография и ключи изолированы — меньше attack surface, проще аудит и сертификация.

**Q (M5):** Зачем polling worker (T-Bank GetState)?  
**A:** Провайдер не всегда шлёт callback вовремя — активная проверка «зависших» платежей.

**Q (M5):** Flipt feature flags — зачем?  
**A:** Gradual rollout, A/B, kill switch без передеплоя — client-side evaluation снижает latency.

**Q (M5):** Circuit breaker — когда срабатывает?  
**A:** При серии ошибок внешнего сервиса — «размыкает» цепь, не каскадирует сбой на всю систему.

**Q (M5):** Timeout и retry для HTTP-клиента?  
**A:** Timeout на каждый запрос; retry только для idempotent ops с backoff и jitter.

---

### Вопрос M6 — ClickHouse

**Q (M6):** Зачем ClickHouse для audit-логов?  
**A:** OLAP — быстрые аналитические запросы по большим объёмам логов; MySQL для OLTP не масштабируется на write-heavy audit.

**Q (M6):** OLTP vs OLAP?  
**A:** OLTP (MySQL) — транзакции, CRUD; OLAP (ClickHouse) — агрегации, отчёты, append-heavy аналитика.

**Q (M6):** Почему batch insert в ClickHouse?  
**A:** Оптимизирован под пачки — меньше overhead, лучше сжатие и throughput.

**Q (M6):** Зачем асинхронная запись audit?  
**A:** Не блокировать основной payment flow — fire-and-forget в буфер/очередь → batch в ClickHouse.

---

### Вопрос M7 — Observability (продвинутое)

**Q (M7):** RED metrics — что измерять?  
**A:** Rate (RPS), Errors (доля 5xx), Duration (latency) — на каждый endpoint.

**Q (M7):** USE metrics — что измерять?  
**A:** Utilization, Saturation, Errors — для ресурсов (CPU, memory, connection pool).

**Q (M7):** Зачем distributed tracing?  
**A:** Trace ID через сервисы — видеть полный путь запроса, найти bottleneck в цепочке вызовов.

**Q (M7):** SLO / SLI / error budget?  
**A:** SLI — метрика (availability, latency); SLO — цель (99.9%); error budget — сколько ошибок можно «потратить» до нарушения SLO.

**Q (M7):** Почему не логировать PII?  
**A:** GDPR, утечки в log aggregation, compliance — correlation ID вместо email/phone в логах.

**Q (M7):** Alertmanager — зачем routing и silencing?  
**A:** Разные severity → разные каналы (PagerDuty vs Slack); silence на время maintenance.

---

### Вопрос M8 — Инфраструктура и DevOps

**Q (M8):** Liveness vs readiness probe?  
**A:** Liveness — перезапуск при зависшем процессе; readiness — убрать из балансировки, пока не готов (DB, миграции).

**Q (M8):** Kustomize base + overlays?  
**A:** Общий base + overlay на окружение (dev/djem/prod) — DRY для K8s-манифестов.

**Q (M8):** Argo Rollouts canary — суть?  
**A:** Постепенный процент трафика на новую версию + анализ метрик → auto-promote или rollback.

**Q (M8):** GitLab CI stages для Go-сервиса?  
**A:** build → test → lint → deploy; artifacts (бинарник/образ), cache (go mod), rules по веткам.

**Q (M8):** Зачем non-root user в Dockerfile?  
**A:** Principle of least privilege — компрометация контейнера не даёт root на node.

**Q (M8):** Requests vs limits в K8s?  
**A:** Requests — гарантированные ресурсы для scheduling; limits — максимум (OOMKill при превышении memory).

**Q (M8):** HPA — когда масштабировать?  
**A:** По CPU/memory или custom metrics — автоматически добавлять/убирать pods при нагрузке.

---

### Вопрос M9 — Безопасность

**Q (M9):** Структура JWT?  
**A:** `header.payload.signature` — claims (sub, exp, roles) в payload; подпись проверяет целостность.

**Q (M9):** Paseto vs JWT?  
**A:** Paseto — безопасные defaults, нет алгоритмической путаницы (alg:none); JWT — шире экосистема.

**Q (M9):** Token rotation и refresh tokens?  
**A:** Короткий access token + долгий refresh; rotation — новый refresh при каждом обновлении, инвалидация украденного.

**Q (M9):** SQL injection prevention?  
**A:** Только prepared statements / parameterized queries; никогда конкатенация SQL со строками пользователя.

**Q (M9):** Зачем mTLS между сервисами?  
**A:** Взаимная аутентификация по сертификатам — не только «клиент доверяет серверу», но и наоборот.

**Q (M9):** WAF/ModSecurity на Ingress?  
**A:** Фильтрация известных атак (SQLi, XSS) на границе до приложения.

**Q (M9):** OWASP API Top 3 риска?  
**A:** Injection, broken authentication, sensitive data exposure — проверять на каждом новом endpoint.

---

### Вопрос M10 — Архитектура

**Q (M10):** Где проходит transactional boundary?  
**A:** В Processor — одна бизнес-операция = одна TX; Storage не открывает TX сам.

**Q (M10):** Domain invariants — пример?  
**A:** `Money` не может быть отрицательным, `PaymentStatus` — только допустимые переходы; правила в domain-типах.

**Q (M10):** Dependency inversion в Go?  
**A:** Processor зависит от `Storer interface`, не от `*MySQLStorage` — подмена в тестах и смена реализации.

**Q (M10):** Multi-replica safety — что запрещено?  
**A:** In-memory locks, local cache без invalidation — состояние только в БД/Redis/Kafka.

**Q (M10):** Зачем ADR?  
**A:** Architecture Decision Records — фиксировать «почему выбрали X, а не Y» для будущей команды.

**Q (M10):** Event-driven vs синхронный вызов?  
**A:** Событие — слабая связность, eventual consistency; синхрон — когда нужен немедленный ответ или strong consistency.

**Q (M10):** Error handling strategy на уровне сервиса?  
**A:** Typed errors в domain → wrapping в Processor → маппинг в HTTP/gRPC codes в API layer.

---

### Вопрос M11 — Тестирование (продвинутое)

**Q (M11):** Зачем mockery v3?  
**A:** Генерация моков из интерфейсов по `.mockery.yaml` — не писать вручную десятки методов.

**Q (M11):** Integration tests с testcontainers?  
**A:** `go test -tags=integration` + реальный MySQL в Docker — проверка SQL, миграций, TX без моков.

**Q (M11):** Contract tests для Kafka?  
**A:** Проверка schema compatibility при изменении Avro — consumer не сломается на новом producer.

**Q (M11):** Coverage gates в CI?  
**A:** Минимальный % покрытия для merge — не панацея, но блокирует полное отсутствие тестов.

**Q (M11):** Как диагностировать flaky test?  
**A:** `-count=100`, `-race`, изоляция времени/сети, детерминированные fixtures, no `time.Sleep` без причины.

**Q (M11):** Benchmarks в CI — зачем?  
**A:** Regression detection — алерт, если latency/alloc выросли после изменения.

---

### Вопрос M12 — Soft skills (Middle)

**Q (M12):** Как проводить код-ревью?  
**A:** Конструктивно: что не так, почему, предложение; разделять «must fix» и «nit»; хвалить хорошие решения.

**Q (M12):** Как менторить джуниора?  
**A:** Объяснять «почему», pair programming, давать растущую по сложности задачи, не делать за него.

**Q (M12):** Оценка сроков ±20% — как?  
**A:** Декомпозиция, буфер на ревью/тесты/неизвестное, опора на похожие задачи из прошлого.

**Q (M12):** Аргументация технического решения?  
**A:** Trade-offs + бизнес-контекст: «выбрали outbox, потому что нужна гарантия доставки без 2PC».

**Q (M12):** Что писать в ADR/README/API-документации?  
**A:** Контекст, решение, альтернативы, последствия — чтобы новый разработчик понял без устных объяснений.

**Q (M12):** «Владеть фичей целиком» — что включает?  
**A:** Проектирование, реализация, тесты, деплой, мониторинг, инциденты, документация, handoff.

---

## Связанные заметки

- MOC: [[00-MOC-Backend-Review]]
- Матрица: [[Требования к грейдам Backend Go-разработчика]]
- Глоссарий: [[00-Глоссарий-фронт→бэк]]
- Capstone: [[10-Capstone-проект]]
