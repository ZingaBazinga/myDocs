---
tags: [junior, http, rest, chi, backend]
---

# HTTP REST (chi) — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#HTTP REST (chi)]]

---

## HTTP: методы и коды ответов

| Метод | Назначение | Идемпотентность |
|-------|------------|-----------------|
| GET | Получить ресурс | Да |
| POST | Создать | Нет |
| PUT | Полная замена | Да |
| PATCH | Частичное обновление | Нет* |
| DELETE | Удалить | Да |

Коды: `200 OK`, `201 Created`, `204 No Content`, `400 Bad Request`, `404 Not Found`, `409 Conflict`, `500 Internal Server Error`.

REST — ресурсы (`/notes`, `/notes/{id}`), не действия (`/createNote`).

---

## `net/http`: Handler и Request

```go
func hello(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusOK)
    _ = json.NewEncoder(w).Encode(map[string]string{"msg": "ok"})
}

http.HandleFunc("/hello", hello)
http.ListenAndServe(":8080", nil)
```

Ключевые типы:
- `http.Handler` — интерфейс `ServeHTTP(w, r)`
- `http.HandlerFunc` — адаптер функции к Handler
- `http.ResponseWriter` — заголовки и тело ответа
- `*http.Request` — метод, URL, заголовки, тело, `r.Context()`

---

## chi router

```go
r := chi.NewRouter()
r.Use(middleware.Logger)
r.Use(middleware.Recoverer)

r.Route("/api/v1", func(r chi.Router) {
    r.Get("/notes", listNotes)
    r.Post("/notes", createNote)
    r.Get("/notes/{id}", getNote)
    r.Put("/notes/{id}", updateNote)
    r.Delete("/notes/{id}", deleteNote)
})
```

URL-параметр: `id := chi.URLParam(r, "id")`.

**Middleware chain:** `r.Use()` регистрирует middleware сверху вниз; запрос идёт вниз по цепочке, ответ — вверх.

```go
func requestID(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        id := uuid.New().String()
        ctx := context.WithValue(r.Context(), ctxKeyRequestID, id)
        next.ServeHTTP(w, r.WithContext(ctx))
    })
}
```

---

## JSON API

```go
func createNote(w http.ResponseWriter, r *http.Request) {
    var req CreateNoteRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        http.Error(w, "invalid json", http.StatusBadRequest)
        return
    }
    if req.Title == "" {
        http.Error(w, "title required", http.StatusBadRequest)
        return
    }
    // ...
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusCreated)
    _ = json.NewEncoder(w).Encode(note)
}
```

Валидация Junior: обязательные поля, базовые типы, понятные 4xx.

---

## Request context

```go
ctx := r.Context() // для таймаутов и отмены
ctx, cancel := context.WithTimeout(ctx, 3*time.Second)
defer cancel()
note, err := processor.GetNote(ctx, id)
```

Данные request-scoped (request ID, user ID) — через `context.WithValue` (осторожно: типизированные ключи).

---

## CORS

Нужен, когда фронт на другом origin обращается к API. Preflight — `OPTIONS` с `Access-Control-Request-Method`.

```go
w.Header().Set("Access-Control-Allow-Origin", "https://app.example.com")
w.Header().Set("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
```

На Junior — понимать зачем; в проде — middleware или Ingress.

---

## Rate limiting

Ограничивает частоту запросов (защита от abuse). Реализуется middleware (token bucket, per-IP). На Junior — знать зачем, не обязательно писать с нуля.

---

## Health checks и несколько серверов

Типичная схема в одном процессе:
- `:8080` — Public API
- `:8081` — `/metrics`, `/health`, `/ready`
- `:9090` — gRPC (см. [[03-gRPC-Junior]])

```go
healthMux := http.NewServeMux()
healthMux.HandleFunc("/health", func(w http.ResponseWriter, r *http.Request) {
    w.WriteHeader(http.StatusOK)
})
go http.ListenAndServe(":8081", healthMux)
```

**Liveness** — процесс жив. **Readiness** — готов (DB подключена, Kafka доступна).

---

## HTTPS и TLS

В проде TLS терминируется на Ingress/nginx; приложение часто слушает HTTP внутри кластера. Junior: понимать, что HTTPS шифрует трафик, сертификаты проверяются клиентом.

---

## Частые ошибки

| Ошибка | Правильно |
|--------|-----------|
| Бизнес-логика и SQL прямо в handler | Handler → Processor → Storage |
| Забыть `Content-Type: application/json` | Всегда выставлять для JSON |
| 500 на ошибку валидации | 400 Bad Request |
| Не читать `r.Body` / не закрывать | `defer r.Body.Close()` при необходимости |
| Глобальный state между запросами | Stateless — данные в DB/кэше с invalidation |

---

## Как проверить, что понял

- [ ] CRUD `/notes` на chi с middleware logging + recovery
- [ ] Тест handler через `httptest.NewRecorder` (см. [[09-Тестирование-SoftSkills-Junior]])
- [ ] Отдельный `/health` на другом порту
- [ ] Передача `r.Context()` в слой Processor

**Связанные темы:** [[06-Архитектура-Junior]] · [[03-gRPC-Junior]] · [[07-Observability-Junior]]
