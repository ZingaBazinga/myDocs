---
tags: [junior, golang, backend]
---

# Go — язык и runtime — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#Go — язык и runtime]]

---

## Go для тех, кто знает другой язык: главные отличия

| Привычка из JS/Python | Идиома Go |
|-----------------------|-----------|
| `try/catch` | `if err != nil { return err }` |
| Классы и наследование | `struct` + composition + interfaces |
| `null` / `None` | `nil` + явные nullable-типы (`sql.NullString`) |
| `async/await` | goroutines + channels + `context` |
| Исключения везде | `panic` только на границах (middleware), не в бизнес-логике |
| `npm install` | `go mod init`, `go get`, `go mod tidy` |

> [!tip] Первое правило Go
> Не борись с языком — принимай явность: типы, ошибки, простые интерфейсы.

---

## Типы данных: примитивы и составные

**Примитивы:** `bool`, `string`, числовые (`int`, `int64`, `float64`, …), `byte` (= `uint8`), `rune` (= `int32`, Unicode code point).

**Слайсы** — динамический массив. Ключевые операции: `append`, `copy`, `len`, `cap`, срезы `s[low:high]`.

```go
nums := []int{1, 2, 3}
nums = append(nums, 4)
sub := nums[1:3] // [2, 3]
```

**Мапы** — хеш-таблица. Создание через `make`, проверка ключа через «comma ok»:

```go
m := make(map[string]int)
m["a"] = 1
v, ok := m["b"] // v=0, ok=false
```

**Struct** — агрегат полей. Встраивание (embedding) вместо наследования:

```go
type User struct {
    ID   int64
    Name string
}

type Admin struct {
    User          // embedded — методы User доступны на Admin
    Permissions []string
}
```

Теги полей (`json:"name"`) используются для сериализации.

---

## Указатели: когда `*` и `&`

- `&x` — взять адрес
- `*p` — разыменовать
- `nil` — нулевое значение указателя

**Value receiver** — метод работает с копией (не меняет оригинал).  
**Pointer receiver** — метод меняет struct или избегает копирования больших структур.

```go
func (u *User) Rename(name string) { u.Name = name } // pointer receiver
```

Правило Junior: если метод меняет receiver — pointer receiver.

---

## Интерфейсы: неявная реализация

В Go интерфейс реализуется автоматически, если тип имеет все методы. Не пиши `implements`.

```go
type Storer interface {
    Save(ctx context.Context, note Note) error
}

// *MySQLStorage автоматически реализует Storer, если есть метод Save
```

- **Type assertion:** `v, ok := x.(Storer)`
- **Type switch:** `switch v := x.(type) { case string: ... }`
- Пустой интерфейс `interface{}` / `any` — любой тип (используй осторожно)

> [!warning] Accept interfaces, return structs
> Функции принимают интерфейсы (для тестируемости), возвращают конкретные типы.

---

## Обработка ошибок

`error` — это значение, не исключение.

```go
if err != nil {
    return fmt.Errorf("save note: %w", err) // %w — wrapping
}

if errors.Is(err, sql.ErrNoRows) { ... }
var e *MyError
if errors.As(err, &e) { ... }
```

**Частые ошибки:**
- Игнорировать `err` через `_`
- Использовать `panic` вместо `return err` в бизнес-логике
- Терять цепочку ошибок без `%w`

**Как проверить, что понял:** напиши функцию, которая оборачивает ошибку и проверяет её через `errors.Is`.

---

## `defer`, `panic`, `recover`

`defer` откладывает вызов до выхода из функции (LIFO — последний defer выполнится первым).

```go
f, err := os.Open("file.txt")
if err != nil { return err }
defer f.Close() // закроется при любом return
```

`panic` — аварийная остановка. `recover` — только внутри `defer`, обычно в HTTP middleware.

---

## `context.Context`

Передаётся **первым аргументом** (после receiver): `func Do(ctx context.Context, ...)`.

```go
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

select {
case <-ctx.Done():
    return ctx.Err()
case result := <-ch:
    return result
}
```

Используется для: отмены, таймаутов, request-scoped данных (request ID).

---

## Goroutines и channels

**Конкурентность** — структура программы; **параллелизм** — одновременное выполнение на разных ядрах.

```go
go func() { ... }() // запуск goroutine

ch := make(chan int)       // небуферизованный
buf := make(chan int, 10)  // буферизованный

ch <- 42    // отправка
v := <-ch   // получение
close(ch)   // закрытие (только отправитель)
for v := range ch { ... } // чтение до закрытия
```

**Частые ошибки:**
- Утечка goroutine (нет получателя на канале)
- Закрытие канала не той стороной
- Race condition без синхронизации (на Junior — понимать концепт; на Middle — `sync.Mutex`, `-race`)

---

## JSON, строки, время

```go
type Note struct {
    ID    int64  `json:"id"`
    Title string `json:"title"`
}

data, err := json.Marshal(note)
err = json.Unmarshal(data, &note)

now := time.Now()
d, _ := time.Parse(time.RFC3339, "2026-01-15T10:00:00Z")
```

Строки в Go — immutable. `[]byte` vs `string`: для HTTP/JSON часто `[]byte`, для текста — `string`. Руны — для Unicode.

---

## Пакеты, модули, layout проекта

```bash
go mod init github.com/you/notes
go get github.com/go-chi/chi/v5
go mod tidy
```

Стандартный layout:
```
cmd/notes/main.go     # точка входа
internal/             # приватный код пакета (не импортируется извне)
pkg/                  # публичные библиотеки (если нужны)
```

- **Exported** — имя с большой буквы (`Save`)
- **Unexported** — с маленькой (`save`)

---

## Сборка и запуск

```bash
go run ./cmd/notes
go build -o bin/notes ./cmd/notes
go test ./...
CGO_ENABLED=0 go build  # статическая линковка для Docker
```

---

## Частые ошибки Junior

| Ошибка | Правильно |
|--------|-----------|
| Цикл `for range` + goroutine с переменной цикла | Передавать копию: `go func(i int) { ... }(i)` |
| Сравнение `err == sql.ErrNoRows` после wrap | `errors.Is(err, sql.ErrNoRows)` |
| Глобальные переменные для DB/логгера | DI через конструкторы |
| `interface{}` везде | Конкретные типы и маленькие интерфейсы |

---

## Как проверить, что понял

- [ ] Объясни разницу value vs pointer receiver на примере
- [ ] Напиши интерфейс `Storer` и две реализации: in-memory и заглушка
- [ ] Обработай ошибку с `%w` и найди её через `errors.Is`
- [ ] Запусти 3 goroutine, собери результаты через channel + `sync.WaitGroup`
- [ ] Распарси и собери JSON struct с тегами

**Следующая тема:** [[02-HTTP-REST-chi-Junior]] · **Инструменты:** [[08-Инструменты-окружение-Junior]]
