---
tags: [middle, vite, testing, build]
---

# Сборка и тестирование — Middle

---

## Vite

**Dev:** нативный ESM, зависимости предсобираются esbuild → быстрый старт.

**Prod:** Rollup — tree-shaking, chunks.

**Конфиг:** `plugins`, **`define`** для констант сборки, **`optimizeDeps`** — предбандлинг зависимостей для dev.

---

## Rollup, esbuild, SWC

- **Rollup** — лучше для библиотек и SPA production (tree-shaking).
- **esbuild / SWC** — экстремально быстрый транспил/минификация; SWC часто в Next.

---

## Tree shaking

Работает с **ESM** статическим импортом; **CommonJS** хуже анализируется. В `package.json` **`"sideEffects": false`** помогает удалить неиспользуемые модули (осторожно если есть глобальные побочные эффекты при импорте).

---

## Code splitting

Динамический **`import()`** создаёт отдельный chunk; именование через комментарии сборки (`webpackChunkName` в webpack-мире; в Vite — `rollupOptions.output.manualChunks`).

Цели: меньше начальный bundle, быстрее TTI.

---

## Source maps

**eval-cheap** vs **full source maps** — баланс скорости сборки и точности отладки. В проде часто не выкладывают публично или заливают в Sentry отдельно.

---

## PostCSS

**Autoprefixer**, **cssnano**; кастомные плагины для дизайн-токенов и т.д.

---

## Monorepo

**pnpm/yarn/npm workspaces** — один репозиторий, несколько пакетов. **Nx / Turborepo** — кэш задач, affected commands, оркестрация CI.

---

## Environment variables

**`.env`**, **`import.meta.env`** в Vite; префикс **`VITE_`** для публичных клиентских переменных. Секреты не попадают в клиентский бандл.

---

## Bundle analysis

`rollup-plugin-visualizer` / vite-plugin — где вес: чаще всего графики библиотек и дубликаты.

---

## Тестовая пирамида

Много **unit** (быстро, дёшево), меньше **integration**, ещё меньше **E2E** (дорого, медленно). Trade-off: уверенность vs скорость CI.

---

## Snapshot-тесты

Полезны для стабильных UI-компонентов и сериализуемых структур; вредны если снимают всё подряд и обновляются без ревью смысла.

---

## Покрытие (coverage)

**Statements / branches / functions / lines** — ветвления часто важнее строк; 100% не гарантирует качество.

---

## Testing Library + Playwright (из матрицы)

**Testing Library** — тестируй как пользователь: роли, лейблы, не детали реализации.

**Playwright** — E2E: устойчивые селекторы, fixtures, параллель, trace при падении.

---

## renderHook

Тестирование кастомных хуков в изоляции с обёрткой провайдеров.

---

## MSW

Перехват сети на уровне Service Worker (браузер) или сервера (Node) — реалистичные интеграционные тесты без реального API.

---

## Асинхронные тесты

**Fake timers** для debounce; **`waitFor`** / **`findBy`**; React **`act`** при обновлении состояния из теста.

---

## Storybook interaction tests

Проверки внутри stories — живая документация + регрессия UI.

---

## Contract testing (Pact)

Потребитель и провайдер согласуют контракт API; ловит breaking changes до продакшена.

---

## Accessibility: Lighthouse

Автоаудит a11y не заменяет ручную проверку с клавиатуры и скринридером.

---

## Связанные заметки

- [[07-A11y-Производительность-Безопасность-Middle]]
- [[05-React-State-Network-Middle]]
