---
tags: [junior, kafka, messaging, backend]
---

# Kafka и Messaging — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#Messaging (Kafka)]]

---

## Базовые понятия Kafka

| Термин | Смысл |
|--------|-------|
| **Topic** | Логическая очередь/поток событий (`note.created`) |
| **Partition** | Шард topic; порядок гарантирован внутри partition |
| **Offset** | Позиция сообщения в partition |
| **Broker** | Сервер Kafka |
| **Consumer group** | Группа consumer'ов делят partitions между собой |

Producer пишет в topic → Consumer читает и commit'ит offset.

---

## Producer и Consumer (концепт)

```go
// Упрощённо — franz-go
client, _ := kgo.NewClient(
    kgo.SeedBrokers("localhost:9092"),
    kgo.ConsumerGroup("notes-service"),
    kgo.ConsumeTopics("note.events"),
)

for {
    fetches := client.PollFetches(ctx)
    fetches.EachRecord(func(r *kgo.Record) {
        // обработка r.Value
        // после успеха — commit offset
    })
}
```

Junior: понимать цикл poll → process → commit, не обязательно знать все опции клиента.

---

## Consumer group и rebalance

Несколько реплик сервиса в одной consumer group — каждая partition обрабатывается одним consumer. При добавлении/удалении пода — **rebalance** (перераспределение partitions).

Stateless: любой под может обработать любое сообщение (при правильном ключе partition).

---

## At-least-once delivery

Kafka гарантирует **как минимум одну** доставку. Сообщение может прийти дважды (retry, rebalance до commit).

**Решение:** идемпотентная обработка — проверка «уже обработано» по event ID или upsert.

```go
func (h *Handler) Handle(ctx context.Context, event NoteCreated) error {
    if h.store.IsProcessed(ctx, event.ID) {
        return nil // дубликат — ок
    }
    // бизнес-логика
    return h.store.MarkProcessed(ctx, event.ID)
}
```

---

## DLQ (Dead Letter Queue)

Отдельный topic для «битых» сообщений после N неудачных попыток. Позволяет не блокировать основной поток и разобрать проблемы вручную.

---

## Transactional Outbox (паттерн)

Проблема: нужно атомарно записать в БД и отправить в Kafka — прямой dual-write ненадёжен.

```
1. BEGIN TX
2. INSERT INTO notes ...
3. INSERT INTO outbox (event_type, payload) ...
4. COMMIT
5. Outbox worker читает outbox → публикует в Kafka → помечает sent
```

Junior: понимать **зачем**, реализация с `SKIP LOCKED` — Middle.

---

## franz-go и apm-queue

- **franz-go** (`twmb/franz-go`) — Kafka-клиент для Go
- **apm-queue** (`git.indels.tech/Drivee/apm-queue/v2`) — обёртка команды с конфигурацией, метриками, DLQ

На Junior: знать, что в проекте используется обёртка; уметь прочитать конфиг consumer/producer.

---

## Частые ошибки

| Ошибка | Правильно |
|--------|-----------|
| Commit до успешной обработки | Сначала process, потом commit |
| Нет идемпотентности | Дубликаты ломают данные |
| Публикация в Kafka до commit БД | Outbox pattern |
| In-memory offset | Commit в Kafka / БД |

---

## Как проверить, что понял

- [ ] Объяснить topic, partition, offset на примере `note.created`
- [ ] Нарисовать схему outbox: HTTP → DB + outbox → worker → Kafka
- [ ] Объяснить, почему consumer должен быть идемпотентным

**Связанные темы:** [[04-MySQL-database-sql-Junior]] · [[06-Архитектура-Junior]]
