---
tags: [performance-review, backend, golang, junior, obsidian]
aliases: [Backend Review Prep]
---

# Backend Review: Junior

Матрица компетенций: [[Требования к грейдам Backend Go-разработчика]]

Нумерация в этой папке — путь к Junior:
- `01-09` — темы Junior
- `10` — capstone-проект и чеклист готовности

## Быстрый старт

| Что | Заметка |
|-----|---------|
| Глоссарий (фронт → бэк) | [[00-Глоссарий-фронт→бэк]] |
| Карточки Q/A (все 22 вопроса) | [[Flashcards-All-Questions]] |
| Capstone | [[10-Capstone-проект]] |

## Junior — темы

| # | Тема | Заметка | Фаза |
|---|------|---------|------|
| 01 | Go — язык и runtime | [[01-Go-язык-Junior]] | 0 |
| 02 | HTTP REST (chi) | [[02-HTTP-REST-chi-Junior]] | 1 |
| 03 | gRPC | [[03-gRPC-Junior]] | 3 |
| 04 | Базы данных (MySQL) | [[04-MySQL-database-sql-Junior]] | 2 |
| 05 | Messaging (Kafka) | [[05-Kafka-Messaging-Junior]] | 3 |
| 06 | Архитектура приложения | [[06-Архитектура-Junior]] | 2–5 |
| 07 | Observability | [[07-Observability-Junior]] | 4 |
| 08 | Инструменты и окружение | [[08-Инструменты-окружение-Junior]] | 0, 4 |
| 09 | Тестирование и soft skills | [[09-Тестирование-SoftSkills-Junior]] | 0–5 |

---

## Программа по фазам (≈ 12 недель)

| Фаза | Недели | Цель | Заметки |
|------|--------|------|---------|
| 0 | 1–2 | Идиоматичный Go, модули, ошибки, тесты | `01`, `08`, `09` |
| 1 | 3–4 | REST API на chi, middleware, health | `02` |
| 2 | 5–6 | MySQL, Storage-слой, слоистая архитектура | `04`, `06` |
| 3 | 7–8 | gRPC, Kafka, события | `03`, `05` |
| 4 | 9–10 | Логи, метрики, Docker, ENV | `07`, `08` |
| 5 | 11–12 | Capstone «notes с событиями» | `10` |

---

## Как готовиться

1. Если переходишь с фронта — начни с [[00-Глоссарий-фронт→бэк]].
2. Пройди `01-09`, проговаривая ответы вслух.
2. Решай карточки из [[Flashcards-All-Questions]] без подглядывания.
3. Собери capstone по чеклисту в [[10-Capstone-проект]].
4. Перед ревью подготовь 3 кейса: техника, процесс, влияние на команду.

---

## Внешние ресурсы

- [A Tour of Go](https://go.dev/tour/)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go by Example](https://gobyexample.com/)
- [[Курсы]] — Hexlet
