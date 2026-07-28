---
tags: [junior, mysql, database, sql, backend]
---

# MySQL и database/sql — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#Базы данных (MySQL)]]

---

## SQL: минимум для Junior

```sql
SELECT id, title, created_at FROM notes WHERE user_id = ? ORDER BY created_at DESC LIMIT 10;
INSERT INTO notes (title, body) VALUES (?, ?);
UPDATE notes SET title = ? WHERE id = ?;
DELETE FROM notes WHERE id = ?;

SELECT n.*, u.name FROM notes n
INNER JOIN users u ON n.user_id = u.id
LEFT JOIN tags t ON ...
```

- `WHERE` — фильтрация
- `JOIN` — связь таблиц (INNER, LEFT)
- `ORDER BY`, `LIMIT` — сортировка и пагинация

---

## `database/sql`: подключение и запросы

```go
import _ "github.com/go-sql-driver/mysql"

dsn := "user:pass@tcp(localhost:3306)/notes?parseTime=true"
db, err := sql.Open("mysql", dsn)
if err != nil { return err }
db.SetMaxOpenConns(25)
db.SetMaxIdleConns(5)

row := db.QueryRowContext(ctx, "SELECT id, title FROM notes WHERE id = ?", id)
var note Note
err = row.Scan(&note.ID, &note.Title)
if errors.Is(err, sql.ErrNoRows) {
    return ErrNotFound
}

rows, err := db.QueryContext(ctx, "SELECT id, title FROM notes")
defer rows.Close()
for rows.Next() {
    if err := rows.Scan(&note.ID, &note.Title); err != nil { return err }
}
return rows.Err()
```

`sql.Open` не подключается сразу — первое обращение проверяет соединение.

---

## Prepared statements

```go
stmt, err := db.PrepareContext(ctx, "INSERT INTO notes (title) VALUES (?)")
defer stmt.Close()
_, err = stmt.ExecContext(ctx, title)
```

Плейсхолдеры `?` — защита от SQL injection. **Никогда** не конкатенировать SQL со строками пользователя.

---

## Транзакции

```go
tx, err := db.BeginTx(ctx, nil)
if err != nil { return err }
defer tx.Rollback() // no-op после Commit

_, err = tx.ExecContext(ctx, "INSERT INTO notes ...")
if err != nil { return err }

_, err = tx.ExecContext(ctx, "INSERT INTO outbox ...")
if err != nil { return err }

return tx.Commit()
```

`defer tx.Rollback()` — идиома: откат при любом `return` до `Commit`.

---

## Nullable-типы

В SQL `NULL` ≠ zero value в Go.

```go
var title sql.NullString
row.Scan(&title)
if title.Valid {
    note.Title = title.String
}
```

Аналоги: `sql.NullInt64`, `sql.NullTime`, `sql.NullBool`.

---

## Connection pool

| Параметр | Зачем |
|----------|-------|
| `SetMaxOpenConns` | Лимит одновременных соединений |
| `SetMaxIdleConns` | Соединения в пуле «наготове» |

Без лимита — исчерпание соединений на MySQL. Junior: понимать зачем, не обязательно тюнить.

---

## Storage-слой

**Только SQL/CRUD, без бизнес-логики.**

```go
type NoteStorage struct {
    db *sql.DB
}

func NewNoteStorage(db *sql.DB) *NoteStorage {
    return &NoteStorage{db: db}
}

func (s *NoteStorage) GetByID(ctx context.Context, id int64) (domain.Note, error) {
    // только SQL + маппинг в domain.Note
}
```

Бизнес-правила («нельзя удалить опубликованную заметку») — в Processor, не здесь.

---

## Миграции Liquibase

Changelog в `deployments/changelogs/`. Junior:
- читать changeset (что меняется в схеме)
- понимать, что миграции применяются до деплоя или при старте (по процессу команды)
- не править уже применённые changeset в проде

---

## Индексы

Индекс ускоряет `WHERE` / `JOIN` по колонке. Без индекса на `user_id` — full table scan. Junior: понимать зачем, без глубокого EXPLAIN (это Middle).

---

## Частые ошибки

| Ошибка | Правильно |
|--------|-----------|
| `db.Query` без `rows.Close()` | Утечка соединений |
| Сравнение `err == sql.ErrNoRows` после wrap | `errors.Is` |
| SQL в HTTP handler | Storage-слой |
| `SELECT *` везде | Явные колонки |
| Забыть `parseTime=true` в DSN | Для `time.Time` в Scan |

---

## Как проверить, что понял

- [ ] CRUD для `notes` через `database/sql`
- [ ] Транзакция: insert note + insert outbox, rollback при ошибке
- [ ] Storage без `if title == ""` — валидация в Processor
- [ ] Обработка `sql.ErrNoRows` → 404 в API

**Связанные темы:** [[06-Архитектура-Junior]] · [[05-Kafka-Messaging-Junior]]
