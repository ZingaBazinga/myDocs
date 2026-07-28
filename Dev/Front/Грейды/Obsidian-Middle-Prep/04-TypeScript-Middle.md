---
tags: [middle, typescript, types]
---

# TypeScript — продвинутый (Middle)

База: [[09-Конспект-Junior-база#TypeScript]]. Ниже — то, что отличает Middle.

---

## Generics

Параметры типов для функций, классов, интерфейсов: `function identity<T>(x: T): T`.

**Ограничения:** `<T extends { id: string }>` — внутри можно безопасно читать `id`.

**Значения по умолчанию:** `<T = unknown>`.

---

## Utility types

- **Partial / Required / Readonly** — модификаторы всех ключей.
- **Pick / Omit** — подмножество ключей.
- **Record<K, V>** — объект с ключами K.
- **Extract / Exclude** — фильтрация union.
- **NonNullable** — убирает null/undefined.
- **ReturnType / Parameters** — из типа функции.
- **InstanceType** — из типа конструктора.
- **Awaited** — разворачивает Promise в async типах.

Знание имён недостаточно — важно **когда** сокращают дублирование и делают рефакторинг безопаснее.

---

## Conditional types

`T extends U ? X : Y` — условные типы; распределение по union (distributive conditional): `T extends any ? …`

**`infer`** — вывод части типа: `type Foo<T> = T extends Promise<infer R> ? R : T`.

---

## Mapped types

`{ [K in keyof T]?: T[K] }` — опциональность; модификаторы **`+`** / **`-`** для `readonly` и `?`.

---

## Template literal types

`` `${Theme}-${Size}` `` — объединение строковых литералов; полезно для дизайн-токенов и API путей.

---

## Type guards

- **`typeof`** для примитивов
- **`instanceof`** для классов
- **`in`** для различимых объектов
- Пользовательские: `function isUser(x: unknown): x is User`

---

## Discriminated unions

Общее поле-тег: `type Msg = { type: 'a'; foo: number } | { type: 'b'; bar: string }` — сужение по `switch(msg.type)`.

---

## Function overloads

Несколько сигнатур + одна реализация; помогает точному типобезопасному API для разных аргументов.

---

## Module augmentation и declaration merging

**`declare module 'pkg'`** — дополнить типы библиотеки. **Declaration merging** — интерфейсы с тем же именем сливаются (осторожно с глобальным загрязнением).

---

## `declare global`

Расширение глобальных типов (например `Window`).

---

## `.d.ts` файлы

Амбиентные объявления без реализации; `types` в package.json; **`triple-slash`** реже в современных проектах.

---

## strict режим

**`strictNullChecks`** — `null`/`undefined` не всегда совместимы с другими типами.

**`noImplicitAny`** — запрет неявного any.

**`strictFunctionTypes`** — контравариантность для параметров функций (нюансы).

---

## `unknown` vs `any`

**`any`** отключает проверки. **`unknown`** безопасен: перед использованием нужен guard или сужение.

---

## Связанные заметки

- [[05-React-State-Network-Middle]] — типизация хуков и props
- [[06-Сборка-Тестирование-Middle]] — типы в тестах
