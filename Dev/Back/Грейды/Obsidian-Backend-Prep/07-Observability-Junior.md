---
tags: [junior, observability, logging, metrics, backend]
---

# Observability — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#Observability]]

---

## Три столпа observability

| Столп | Инструмент | Зачем |
|-------|------------|-------|
| **Logs** | zap | Что произошло, с контекстом |
| **Metrics** | Prometheus | Сколько, как быстро, сколько ошибок |
| **Traces** | OpenTelemetry (Middle) | Путь запроса через сервисы |

Junior: logs + базовые metrics + health probes.

---

## Structured logging (zap)

```go
log, _ := zap.NewProduction()
defer log.Sync()

log.Info("note created",
    zap.Int64("note_id", note.ID),
    zap.String("request_id", requestID),
)

log.Error("failed to save note",
    zap.Error(err),
    zap.Int64("note_id", note.ID),
    zap.String("operation", "storage.Insert"),
)
```

Уровни: `Debug`, `Info`, `Warn`, `Error`.

**Правила:**
- Ошибки — всегда с контекстом (request ID, user ID, operation)
- Не логировать пароли, токены, PII
- `Debug` — локально; в проде уровень `Info` или выше

---

## Sentry

Отправлять в Sentry **неожиданные** ошибки (5xx, panic после recovery), не каждую 404.

```go
if err != nil {
    log.Error("...", zap.Error(err))
    sentry.CaptureException(err) // только для truly unexpected
    return
}
```

---

## Prometheus: базовые метрики

```go
var (
    httpRequestsTotal = prometheus.NewCounterVec(
        prometheus.CounterOpts{Name: "http_requests_total"},
        []string{"method", "path", "status"},
    )
    httpDuration = prometheus.NewHistogramVec(
        prometheus.HistogramOpts{Name: "http_request_duration_seconds"},
        []string{"method", "path"},
    )
)
```

Типы:
- **Counter** — только растёт (число запросов)
- **Gauge** — текущее значение (размер очереди)
- **Histogram** — распределение (latency)

Endpoint `/metrics` на порту `8081` (отдельно от Public API).

---

## Health и readiness

```go
func readiness(db *sql.DB) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        if err := db.PingContext(r.Context()); err != nil {
            w.WriteHeader(http.StatusServiceUnavailable)
            return
        }
        w.WriteHeader(http.StatusOK)
    }
}
```

| Probe | Вопрос | Действие K8s |
|-------|--------|--------------|
| **Liveness** `/health` | Процесс завис? | Перезапуск пода |
| **Readiness** `/ready` | Можно слать трафик? | Убрать из балансировщика |

---

## Request ID в логах

Middleware генерирует UUID → кладёт в `context` → logger добавляет поле `request_id` → тот же ID в ответе (`X-Request-ID`).

---

## Частые ошибки

| Ошибка | Правильно |
|--------|-----------|
| `fmt.Println` в проде | zap с полями |
| Логировать тело запроса с паролем | Whitelist полей |
| Метрики на `:8080` с API | Отдельный порт `:8081` |
| Readiness всегда 200 | Проверять DB/Kafka |

---

## Как проверить, что понял

- [ ] Middleware: request ID + duration log
- [ ] Counter `http_requests_total` с labels method/path/status
- [ ] `/ready` падает, если MySQL недоступна

**Связанные темы:** [[02-HTTP-REST-chi-Junior]] · [[08-Инструменты-окружение-Junior]]
