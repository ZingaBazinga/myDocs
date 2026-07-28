---
tags: [junior, testing, soft-skills, backend]
---

# Тестирование и Soft Skills — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#Тестирование]] · [[Требования к грейдам Backend Go-разработчика#Soft skills]]

---

## Зачем тесты

| Вид | Что проверяет | Скорость |
|-----|---------------|----------|
| **Unit** | Функция/Processor с моками | Быстро |
| **Integration** | Storage + реальная DB | Медленнее |

Покрытие (`go test -cover`) ≠ качество. Важнее: критичные пути и граничные случаи.

---

## Table-driven tests

```go
func TestValidateTitle(t *testing.T) {
    tests := []struct {
        name  string
        title string
        wantErr bool
    }{
        {"ok", "hello", false},
        {"empty", "", true},
        {"spaces", "   ", true},
    }
    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            t.Parallel()
            err := ValidateTitle(tc.title)
            if tc.wantErr {
                require.Error(t, err)
                return
            }
            require.NoError(t, err)
        })
    }
}
```

`testing.T`: `t.Run()`, `t.Parallel()`, `t.Helper()`.

---

## testify: assert vs require

```go
require.NoError(t, err)   // останавливает тест при ошибке
assert.Equal(t, want, got) // продолжает тест
```

---

## httptest: тест HTTP handler

```go
func TestCreateNote(t *testing.T) {
    proc := NewMockProcessor()
    h := NewHandler(proc)

    body := `{"title":"test"}`
    req := httptest.NewRequest(http.MethodPost, "/notes", strings.NewReader(body))
    rec := httptest.NewRecorder()

    h.CreateNote(rec, req)

    require.Equal(t, http.StatusCreated, rec.Code)
    require.Contains(t, rec.Body.String(), "test")
}
```

---

## Моки через интерфейсы

```go
type MockStorage struct {
    InsertFunc func(ctx context.Context, note domain.Note) error
}

func (m *MockStorage) Insert(ctx context.Context, note domain.Note) error {
    return m.InsertFunc(ctx, note)
}
```

На Middle — генерация через mockery v3. Junior — ручной mock достаточен.

---

## Запуск тестов

```bash
go test ./...
go test -v -run TestCreateNote ./internal/processor/
go test -cover ./...
go test -race ./...
```

---

## Soft skills Junior

| Ожидание | Практика |
|----------|----------|
| Задачи в срок | Дробить на подзадачи, синк при рисках |
| Блокировки до дедлайна | Написать в чат/тикет, не молчать |
| Код-ревью | Принимать feedback, задавать вопросы «почему» |
| Процессы команды | MR, conventional commits, CI |
| Оценка сроков | Для рутинных задач — часы/дни с запасом |
| Заметки | Что сделал, что осталось, что заблокировано |

**Коммуникация:** статус задачи — «сделано / в работе / заблокировано + причина».

---

## Частые ошибки в тестах

| Ошибка | Правильно |
|--------|-----------|
| Тест зависит от порядка | `t.Parallel()` + изоляция |
| Нет subtests | `t.Run(tc.name, ...)` |
| Тестируют implementation details | Тестировать поведение (input → output) |
| Интеграционные тесты без тега | `//go:build integration` |

---

## Как проверить, что понял

- [ ] Table-driven test для валидации
- [ ] httptest для POST `/notes`
- [ ] Mock Storage для Processor unit test
- [ ] `go test -cover` > 0 на processor пакете

**Связанные темы:** [[01-Go-язык-Junior]] · [[02-HTTP-REST-chi-Junior]] · [[10-Capstone-проект]]
