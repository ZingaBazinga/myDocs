## [[Junior]]

### HTML

- Понимание DOCTYPE и режимов рендеринга (quirks mode vs standards mode)
- Семантические теги: `header`, `nav`, `main`, `section`, `article`, `aside`, `footer`, `figure`, `figcaption`, `time`, `mark`, `details`, `summary`
- Формы: типы `input` (text, email, password, number, date, file, checkbox, radio, range, color), `select`, `textarea`, `fieldset`, `legend`, `label`
- Встроенная валидация форм: `required`, `pattern`, `min`, `max`, `minlength`, `maxlength`, `step`
- Мета-теги: `viewport`, `charset`, `description`, `og:*` (Open Graph базовое)
- Понимание блочных и строчных элементов
- Атрибуты `data-*` и работа с ними через JS
- Базовая доступность: `alt` для изображений, структура заголовков `h1`–`h6`, `tabindex`, `aria-label`
- Встраивание медиа: `img`, `video`, `audio`, `picture`, `source`, `srcset`
- Различие между `<div>` и семантическими альтернативами

### CSS

- Блочная модель (box model): `content-box` vs `border-box`
- Позиционирование: `static`, `relative`, `absolute`, `fixed`, `sticky`
- Flexbox: оси, `justify-content`, `align-items`, `flex-grow`, `flex-shrink`, `flex-basis`, `flex-wrap`, `gap`, `order`
- Grid: `grid-template-columns`, `grid-template-rows`, `grid-area`, `fr`, `repeat()`, `minmax()`, `auto-fit`, `auto-fill`
- Селекторы: каскадность, специфичность (расчёт веса), наследование
- Псевдоклассы: `:hover`, `:focus`, `:active`, `:nth-child()`, `:first-child`, `:last-child`, `:not()`, `:focus-visible`, `:focus-within`
- Псевдоэлементы: `::before`, `::after`, `::placeholder`, `::selection`
- Адаптивная вёрстка: `@media` (min-width, max-width, prefers-color-scheme, prefers-reduced-motion)
- Единицы измерения: `px`, `em`, `rem`, `%`, `vw`, `vh`, `dvh`, `svh`, `lvh`, `clamp()`, `min()`, `max()`
- Переменные CSS: `--custom-property`, `var()`, область видимости переменных
- Работа со шрифтами: `@font-face`, `font-display`, подключение Google Fonts, системные шрифты
- Переходы: `transition-property`, `transition-duration`, `transition-timing-function`, `transition-delay`
- Базовые трансформации: `translate`, `scale`, `rotate`
- `overflow`, `text-overflow`, `white-space`
- `z-index` и контексты наложения (stacking context)
- Принципы mobile-first

### JavaScript

- Типы данных: примитивы (`string`, `number`, `bigint`, `boolean`, `null`, `undefined`, `symbol`) и объекты
- Приведение типов: явное и неявное, `==` vs `===`
- Область видимости: `var`, `let`, `const`, лексическое окружение, hoisting
- Замыкания: определение, практическое применение, утечки памяти
- Контекст `this`: правила определения, `call`, `apply`, `bind`, стрелочные функции и `this`
- Прототипное наследование: цепочка прототипов, `__proto__`, `Object.create()`, `Object.getPrototypeOf()`
- Классы ES6: `constructor`, `extends`, `super`, статические методы, приватные поля `#`
- Деструктуризация: объектов, массивов, вложенная, значения по умолчанию, rest-параметры
- Spread/rest: для массивов, объектов, аргументов функций
- Стрелочные функции: синтаксис, отличия от обычных функций
- Шаблонные литералы: интерполяция, tagged templates
- Модули: `import/export`, named/default exports, динамический `import()`
- Итераторы и генераторы: `Symbol.iterator`, `for...of`, `function*`, `yield`
- Map, Set, WeakMap, WeakSet: отличия от объектов и массивов, случаи использования
- Промисы: состояния, цепочки `.then()/.catch()/.finally()`, `Promise.all()`, `Promise.allSettled()`, `Promise.race()`, `Promise.any()`
- `async/await`: обработка ошибок, параллельное выполнение, `for await...of`
- Event Loop: call stack, task queue (macrotasks), microtask queue, порядок выполнения `setTimeout`, `Promise`, `queueMicrotask`
- DOM API: `querySelector`, `querySelectorAll`, `getElementById`, `createElement`, `appendChild`, `removeChild`, `textContent` vs `innerHTML`
- Обработка событий: `addEventListener`, всплытие и перехват, `event.stopPropagation()`, `event.preventDefault()`, делегирование событий
- Работа с массивами: `map`, `filter`, `reduce`, `find`, `findIndex`, `some`, `every`, `flat`, `flatMap`, `sort`, `includes`, `indexOf`
- Работа со строками: `split`, `join`, `slice`, `substring`, `trim`, `padStart`, `padEnd`, `startsWith`, `endsWith`, `replaceAll`
- Работа с объектами: `Object.keys()`, `Object.values()`, `Object.entries()`, `Object.assign()`, `Object.freeze()`, `Object.fromEntries()`
- JSON: `parse`, `stringify`, reviver/replacer, обработка ошибок парсинга
- Таймеры: `setTimeout`, `setInterval`, `clearTimeout`, `clearInterval`, `requestAnimationFrame`
- Обработка ошибок: `try/catch/finally`, создание кастомных ошибок, `Error` типы
- Регулярные выражения: базовый синтаксис, флаги, `test()`, `match()`, `replace()`, `exec()`
- `structuredClone()`, глубокое и поверхностное копирование

### Работа с сетью

- HTTP: методы (GET, POST, PUT, PATCH, DELETE), коды ответов (2xx, 3xx, 4xx, 5xx)
- `fetch()` API: headers, body, методы ответа (`.json()`, `.text()`, `.blob()`)
- Понимание REST: ресурсы, эндпоинты, CRUD-операции
- CORS: что это, почему возникает, simple vs preflight requests
- JSON как формат обмена данными
- Базовое понимание DNS, TCP/IP, HTTPS
- Cookies: `document.cookie`, атрибуты `HttpOnly`, `Secure`, `SameSite`

### Фреймворк (React)

**React:**
- JSX: синтаксис, выражения, условный рендеринг, рендеринг списков и `key`
- Функциональные компоненты
- Хуки: `useState`, `useEffect`, `useContext`, `useRef`, `useMemo`, `useCallback`
- Правила хуков: порядок вызовов, только на верхнем уровне
- Props: передача данных, children, деструктуризация, `defaultProps`
- Подъём состояния (lifting state up)
- Управляемые и неуправляемые компоненты форм
- Обработка событий в React: SyntheticEvent
- Базовый роутинг (Tanstack Router): `Route`, `Link`, `useNavigate`, `useParams`
- Условный рендеринг: тернарный оператор, `&&`, паттерн early return


### TypeScript (базовый)

- Примитивные типы: `string`, `number`, `boolean`, `null`, `undefined`, `void`, `never`, `unknown`, `any`
- Интерфейсы и типы: `interface`, `type`, разница между ними
- Массивы и кортежи: `string[]`, `Array<string>`, `[string, number]`
- Объединение и пересечение типов: `|`, `&`
- Литеральные типы: `'success' | 'error'`
- Enum: числовые и строковые
- Типизация функций: параметры, возвращаемый тип, опциональные параметры, значения по умолчанию
- Type assertion: `as`, `<Type>`
- Типизация объектов: опциональные свойства `?`, `readonly`
- `typeof` и `keyof` на базовом уровне

### Инструменты и окружение

- Git: `init`, `clone`, `add`, `commit`, `push`, `pull`, `branch`, `checkout`/`switch`, `merge`, `log`, `diff`, `stash`
- Разрешение merge-конфликтов
- `.gitignore`: синтаксис и типичные паттерны
- npm/yarn: `install`, `add`, `remove`, `scripts`, `package.json`, `package-lock.json`/`yarn.lock`, semver
- ESLint: использование с готовой конфигурацией, понимание правил, `// eslint-disable`
- Prettier: настройка форматирования, интеграция с редактором
- Chrome DevTools: Elements, Console, Network, Application (Storage)
- VS Code / WebStorm: горячие клавиши, расширения, настройка
- Командная строка: `cd`, `ls`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `grep`, `chmod`, `ssh` (базово)
- Понимание `node_modules`, установка и обновление зависимостей

### Тестирование

- Понимание зачем нужны тесты, виды тестирования (unit, integration, E2E)
- Написание unit-тестов (Jest или Vitest): `describe`, `it`/`test`, `expect`, матчеры (`toBe`, `toEqual`, `toBeTruthy`, `toContain`, `toThrow`)
- Setup/teardown: `beforeEach`, `afterEach`, `beforeAll`, `afterAll`
- Моки: `jest.fn()`, `jest.mock()`, `jest.spyOn()`
- Тестирование компонентов: `@testing-library/react` или `@vue/test-utils` — базовый render, поиск элементов, симуляция событий

### Soft skills (???)

- Выполняет чётко поставленные задачи в срок
- Сообщает о блокировках и трудностях до дедлайна
- Принимает и применяет обратную связь
- Задаёт уточняющие вопросы по задачам
- Следует установленным в команде процессам и соглашениям
- Пишет понятные сообщения коммитов
- Базовая грамотность в деловой переписке
- Оценивает сроки рутинных задач
- Ведёт заметки о проделанной работе

---

## [[Middle]]

*Включает все требования Junior, плюс:*

### HTML

- Микроразметка: Schema.org, JSON-LD, Open Graph, Twitter Cards
- SEO-оптимизация разметки: canonical, hreflang, robots meta, sitemap.xml
- Продвинутые формы: FormData API, загрузка файлов, drag and drop API
- Web Components: `<template>`, `<slot>`, Shadow DOM, Custom Elements, `customElements.define()`
- Content Security Policy через мета-теги
- Атрибуты загрузки: `loading="lazy"`, `decoding="async"`, `fetchpriority`
- `<dialog>` элемент и модальные окна
- Понимание критического пути рендеринга (Critical Rendering Path) в контексте HTML
- `<link rel="preload|prefetch|preconnect|dns-prefetch">`
- Inert-атрибут и управление фокусом

### CSS

- Архитектура CSS: BEM (методология, именование)
- CSS Modules: scoped-стили, `composes`, интеграция с бандлерами
- CSS-in-JS: styled-components, Emotion, Linaria, trade-offs подходов
- Продвинутые анимации: `@keyframes`, `animation-fill-mode`, `will-change`, GPU-ускорение, `transform` и `opacity` как дешёвые свойства
- `requestAnimationFrame` для JS-анимаций
- CSS Container Queries: `@container`, `container-type`, `container-name`
- CSS Cascade Layers: `@layer`, порядок приоритетов
- Subgrid
- Logical properties: `margin-inline`, `padding-block`, `inline-size`, `block-size`
- Scroll Snap: `scroll-snap-type`, `scroll-snap-align`
- `aspect-ratio`
- `:has()` селектор и его применения
- Продвинутые градиенты: `conic-gradient`, `radial-gradient`, множественные фоны
- `@supports` и feature queries
- `writing-mode`, `direction`, поддержка RTL-языков
- Print-стили: `@media print`
- CSS Nesting (нативный)
- View Transitions API (базовое)
- `color-mix()`, `oklch()`, современные цветовые пространства

### JavaScript

- Паттерны проектирования: Module, Observer, Pub/Sub, Factory, Singleton, Strategy, Decorator, Proxy, Iterator, Command
- SOLID-принципы на практике: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
- Принципы DRY, KISS, YAGNI
- Глубокое понимание Event Loop: микротаски vs макротаски, requestAnimationFrame в цикле, requestIdleCallback, queueMicrotask
- Браузерный рендеринг: парсинг HTML → DOM, CSS → CSSOM, Layout → Paint → Composite, reflow и repaint, render-blocking vs parser-blocking ресурсы
- Garbage Collection: алгоритм mark-and-sweep, причины утечек памяти (circular references, замыкания, DOM-ссылки, таймеры, event listeners)
- Proxy и Reflect: перехват операций, реактивные данные, валидация
- Symbol: `Symbol.iterator`, `Symbol.toPrimitive`, `Symbol.hasInstance`, well-known symbols
- WeakRef и FinalizationRegistry
- Web APIs: Intersection Observer, Resize Observer, Mutation Observer, Performance Observer
- Web Workers: создание, `postMessage`, `onmessage`, `SharedWorker`, `Transferable` objects
- WebSocket: создание подключения, обработка событий, reconnect-стратегии
- Server-Sent Events (SSE): `EventSource`, отличия от WebSocket
- IndexedDB: создание БД, object stores, индексы, транзакции, курсоры
- LocalStorage, SessionStorage: ограничения, синхронность, storage event
- Drag and Drop API
- Clipboard API: `navigator.clipboard.read()`, `navigator.clipboard.write()`
- History API: `pushState`, `replaceState`, `popstate`
- File API и Blob: чтение файлов, создание URL, скачивание файлов
- `AbortController` и `AbortSignal`: отмена fetch-запросов, таймауты, комбинирование
- Debounce и throttle: реализация, различия, применение
- Каррирование и частичное применение
- Иммутабельность данных: техники, библиотеки (Immer), `structuredClone`, `Object.freeze` рекурсивный
- Optional chaining `?.` и nullish coalescing `??` — глубокое понимание edge cases
- `Intl` API: `NumberFormat`, `DateTimeFormat`, `RelativeTimeFormat`, `Collator`, `PluralRules`

### TypeScript (продвинутый)

- Generics: функции, классы, интерфейсы, ограничения (`extends`), значения по умолчанию
- Utility types: `Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `Record`, `Extract`, `Exclude`, `NonNullable`, `ReturnType`, `Parameters`, `InstanceType`, `Awaited`
- Conditional types: `T extends U ? X : Y`, `infer`
- Mapped types: `[K in keyof T]`, модификаторы `+`, `-`, `readonly`, `?`
- Template literal types: `` `${string}-${number}` ``
- Type guards: `typeof`, `instanceof`, `in`, `is`, кастомные type guard функции
- Discriminated unions: `type` поле для дискриминации?
- Function overloads
- Module augmentation и declaration merging
- `declare module`, `declare global`
- `.d.ts` файлы: написание и подключение
- `strict` режим: `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes`
- Работа с `unknown` vs `any`: безопасное приведение типов

### Фреймворк (глубокие знания)

**React:**
- Виртуальный DOM: reconciliation algorithm, diffing, fiber architecture (концептуально)
- Порталы: `createPortal`, случаи использования
- Границы ошибок: `ErrorBoundary`, `componentDidCatch`, `getDerivedStateFromError`
- `React.memo`, `useMemo`, `useCallback`: когда использовать, когда не стоит
- `useReducer`: сложные состояния, паттерн reducer
- `useId`, `useDeferredValue`, `useTransition`
- `forwardRef` и `useImperativeHandle`
- Context API: создание, оптимизация ре-рендеров, разделение контекстов
- Suspense и lazy loading: `React.lazy`, `Suspense`, code splitting на уровне компонентов
- React Server Components: концептуальное понимание
- Конкурентный рендеринг: batching, transitions, приоритеты обновлений
- Серверный рендеринг: SSR, SSG, ISR — концепции и отличия
- Tanstack Start / Remix: App Router, Server Actions, файловый роутинг, middleware, API routes

**Vue:**
- Базовые знания

**Angular:**
- Базовые знания

### State Management

- Паттерны управления состоянием: Redux, jotai, zustand, etc
- Нормализация данных в store
- Оптимистичные обновления (optimistic updates)
- Кэширование серверного состояния: TanStack Query, SWR, Apollo Client cache
- Разделение серверного и клиентского состояния
- Finite state machines: XState (концептуально)
- Глобальное vs локальное состояние: критерии выбора

### Работа с сетью (продвинутое)

- REST API: проектирование, версионирование, пагинация (offset, cursor), фильтрация, сортировка
- GraphQL: запросы, мутации, подписки, фрагменты, переменные, Apollo Client / urql
- HTTP/2: мультиплексирование, server push, бинарный протокол, HPACK-сжатие
- HTTP/3 и QUIC: базовое понимание
- Заголовки кэширования: `Cache-Control`, `ETag`, `If-None-Match`, `If-Modified-Since`, `Vary`
- `Content-Type`: `application/json`, `multipart/form-data`, `application/x-www-form-urlencoded`
- Rate limiting и retry-стратегии: exponential backoff, jitter

### Сборка и инструменты

- Vite: архитектура (ESM dev, Rollup prod), конфигурация, plugins, `define`, `optimizeDeps`
- Понимание Rollup, esbuild, SWC — роли и отличия
- Tree shaking: как работает, ES modules vs CommonJS, `sideEffects` в `package.json`
- Code splitting: `import()`, chunk naming, splitChunks
- Source maps: типы, конфигурация для dev/prod
- PostCSS: autoprefixer, cssnano, кастомные плагины
- Monorepo: npm/yarn/pnpm workspaces, Nx, Turborepo — базовое понимание
- Environment variables: `.env` файлы, `process.env`, `import.meta.env`
- Bundle analysis: `vite-plugin-visualizer`

### Тестирование (продвинутое)

- Тестовая пирамида: unit → integration → E2E, соотношение, trade-offs
- Snapshot-тестирование: когда полезно, когда вредно
- Покрытие кода: `coverage`, branches, statements, functions, lines — понимание метрик
- Testing Library: playwright-ts
- Тестирование хуков: `renderHook`
- MSW (Mock Service Worker): мокирование API, handlers, `setupServer`, `setupWorker`
- Тестирование асинхронного кода: таймеры (`jest.useFakeTimers`), промисы, `act()`
- Component testing: Storybook interaction tests
- E2E тестирование: Playwright — базовое написание тестов, selectors, assertions, fixtures
- Contract testing: Pact (понимание)?
- Accessibility testing: Lighthouse

### Доступность (Accessibility)

- ARIA: roles, states, properties, landmarks
- `aria-live` regions: `polite`, `assertive`
- `aria-expanded`, `aria-hidden`, `aria-describedby`, `aria-labelledby`
- Управление фокусом: focus trap, focus restoration, skip links
- Клавиатурная навигация: все интерактивные элементы доступны с клавиатуры
- Screen readers: тестирование с VoiceOver / NVDA, как SR интерпретирует разметку
- Цветовой контраст: соотношения WCAG, инструменты проверки
- Reduced motion: `prefers-reduced-motion`, адаптация анимаций
- High contrast mode: `forced-colors`
- Доступные компоненты: модальные окна, табы, dropdown, автокомплит, date picker

### Производительность

- Core Web Vitals: LCP, FID/INP, CLS — что влияет, как измерять, как улучшать
- Lighthouse: метрики, аудиты, CI-интеграция
- Chrome DevTools Performance: запись профиля, flame chart, long tasks, main thread
- Network panel: waterfall, timing breakdown, throttling
- Critical Rendering Path: оптимизация порядка загрузки CSS/JS
- Оптимизация изображений: WebP, AVIF, responsive images (`srcset`, `sizes`), lazy loading, `<picture>`
- Шрифты: `font-display`, preload, self-hosted vs CDN, subset, variable fonts
- Оптимизация бандла: code splitting, dynamic imports, lazy routes, vendor chunking
- Кэширование: стратегии (`Cache-Control`, content hash в именах файлов), Service Worker cache
- Prefetching и preloading: `<link rel="preload">`, `<link rel="prefetch">`, `<link rel="modulepreload">`
- `requestIdleCallback`, `requestAnimationFrame` — оптимизация выполнения JS
- Виртуализация длинных списков: `@tanstack/virtual`
- Debounce/throttle для обработчиков scroll, resize, input
- Memory leaks: идентификация через DevTools Memory tab, heap snapshots


### Безопасность

- XSS (Cross-Site Scripting): типы (Stored, Reflected, DOM-based), предотвращение, sanitization, DOMPurify
- CSRF (Cross-Site Request Forgery): токены, `SameSite` cookies, заголовки
- Content Security Policy: директивы (`script-src`, `style-src`, `img-src`, `connect-src`), nonce, hash
- CORS: simple requests, preflight, `Access-Control-Allow-*` заголовки, credentials
- Secure cookies: `HttpOnly`, `Secure`, `SameSite`, `__Host-` prefix
- JWT: структура (header.payload.signature), хранение (cookies vs localStorage), refresh tokens, rotation
- HTTPS: TLS handshake, сертификаты, HSTS
- Subresource Integrity (SRI): `integrity` атрибут для CDN-ресурсов
- Open Redirect: валидация URL
- `rel="noopener noreferrer"` для внешних ссылок

### Архитектура

- Feature-Sliced Design: layers, slices, segments
- Clean Architecture: entities, use cases, adapters, infrastructure
- Atomic Design: atoms, molecules, organisms, templates, pages
- Компонентная архитектура: принципы декомпозиции, smart vs dumb components, compound components pattern
- Render props, HOC (Higher Order Components), custom hooks — trade-offs
- Принцип инверсии зависимостей на практике
- Монорепозиторий vs полирепозиторий: trade-offs
- API Layer: абстракция работы с API, interceptors, трансформация данных
- Error boundaries и глобальная обработка ошибок
- Feature flags: реализация, A/B тестирование
- Управление маршрутизацией: nested routes, layouts, guards, code splitting по маршрутам

### DevOps и инфраструктура

- Git: interactive rebase, cherry-pick, bisect, reflog, worktrees, submodules
- Git Flow
- CI/CD: GitLab CI — написание пайплайнов, stages, jobs, artifacts, caching
- Docker: Dockerfile для фронтенд-приложений, multi-stage builds, `.dockerignore`, оптимизация слоёв
- Nginx: конфигурация для SPA (try_files), proxy_pass, gzip, кэширование, SSL, заголовки безопасности
- CDN: принципы работы, настройка (Cloudflare, CloudFront), cache invalidation
- Мониторинг ошибок: Sentry — интеграция, source maps, breadcrumbs, context
- Аналитика: AppsFlyer
- Semantic versioning: major.minor.patch, conventional commits, автогенерация changelog

### Soft skills

- Самостоятельно декомпозирует задачи и выбирает решения
- Проводит качественные код-ревью с конструктивной обратной связью
- Менторит джуниоров: объясняет решения, проводит pair programming
- Управляет своими приоритетами, предлагает корректировки скоупа задач
- Оценивает сроки своих задач и задач джуниоров с точностью ±20%
- Аргументирует технические решения с бизнес-обоснованием
- Участвует в найме: проводит секции технического собеседования
- Чётко коммуницирует статус задач, риски и блокеры
- Пишет техническую документацию: README, ADR, API-документация
- Понимает продуктовый контекст: зачем делается фича, кто пользователь
- Предлагает улучшения в процессах команды
- Владеет целыми фичами от проектирования до деплоя

---

## Senior

*Включает все требования Middle, плюс:*

### Архитектура и системный дизайн

- Проектирование фронтенд-систем с нуля: выбор стека, структуры проекта, инфраструктуры
- Micro-frontends: Module Federation, single-spa, iframe-based, web components-based — trade-offs каждого подхода
- SSR vs CSR vs SSG vs ISR vs Streaming SSR: выбор подхода для конкретного проекта с обоснованием
- BFF (Backend For Frontend): проектирование, когда нужен, реализация на Node.js
- Design Systems: проектирование с нуля, версионирование, документация, governance, multi-brand theming
- Storybook: addon ecosystem, play functions, visual testing, composition
- Модульная архитектура: dependency graphs, circular dependency detection, модульные границы
- Проектирование для масштабируемости: 10x рост пользователей, данных, команды
- API design для фронтенд-библиотек: публичный API, обратная совместимость, deprecation strategy
- Архитектурные паттерны: CQRS на фронтенде, Event Sourcing, Domain-Driven Design
- Edge computing: Cloudflare Workers, Vercel Edge Functions, Deno Deploy — архитектурные решения
- Offline-first архитектура: стратегии синхронизации, конфликты, CRDTs
- Real-time архитектура: WebSocket vs SSE vs polling vs WebRTC — выбор подхода
- Оценка и управление техническим долгом: инвентаризация, приоритизация, roadmap погашения
- Architecture Decision Records (ADR): написание, хранение, ревью
- Миграционные стратегии: Strangler Fig pattern, постепенная миграция фреймворков, feature flags

### JavaScript и TypeScript (экспертный)

- Внутренности V8: JIT-компиляция (Ignition, TurboFan), hidden classes, inline caching, deoptimization
- AST (Abstract Syntax Tree): структура, трансформации, babel plugins, eslint rules на основе AST
- Написание кастомных ESLint правил
- Написание кастомных Babel/SWC плагинов
- Написание кастомных Webpack/Vite плагинов
- Codegen: генерация кода из OpenAPI/Swagger, GraphQL schema
- TypeScript compiler API: программный доступ к компилятору
- Продвинутая система типов: рекурсивные типы, variadic tuple types, distributive conditional types, type-level programming
- `const` assertions и их влияние на inference
- Branded types и nominal typing через intersections
- Type-safe event emitters, state machines, builders
- Метапрограммирование: Proxy, Reflect, декораторы, Symbol

### Производительность (экспертная)

- Performance budgets: настройка, CI enforcement, мониторинг деградации
- Synthetic monitoring: Lighthouse CI, WebPageTest API, SpeedCurve
- Advanced profiling: CPU profiling, memory profiling, heap snapshots, allocation timelines
- Long Tasks API: мониторинг, разбиение задач через `scheduler.yield()`, `scheduler.postTask()`
- Layout thrashing: причины, batch DOM reads/writes, `fastdom`
- Compositor layers: GPU-ускорение, `will-change`, `transform: translateZ(0)`, layer promotion
- Paint и composite: какие свойства триггерят paint, какие — только composite
- Font optimization: `unicode-range`, `size-adjust`, `ascent-override`, `descent-override`, fallback fonts matching
- Resource hints: `<link rel="preload">` vs `<link rel="prefetch">` vs `<link rel="preconnect">` — стратегия использования
- Image optimization pipeline: CDN image processing, responsive breakpoints, art direction
- JavaScript execution: parsing budget, script evaluation cost, hydration cost
- Partial hydration, progressive hydration, islands architecture (Astro)
- Сетевая оптимизация: reducing round trips, connection pooling, resource prioritization
- Third-party script management: async/defer, facade pattern, web worker offloading

### Тестирование (экспертное)

- Стратегия тестирования для проекта: что тестировать, на каком уровне, процент покрытия
- Архитектура тестового фреймворка: custom matchers, custom render wrappers, test utilities
- Integration testing: тестирование целых features end-to-end в компонентных тестах
- E2E: Playwright — Page Object Model, custom commands, CI-интеграция, параллельный запуск, retry strategy
- Performance testing: Lighthouse CI, custom performance assertions
- Load testing фронтенда: k6 browser module
- Chaos engineering: имитация сетевых ошибок, медленного API, offline-режима
- Тестирование в CI: оптимизация времени прогона, шардирование, test impact analysis
- Mutation testing: Stryker — концепция и применение
- Flaky tests: диагностика, изоляция, стратегии устранения
- Test data management: factories, builders, seeding

### Безопасность (экспертная)

- Threat modeling для фронтенд-приложений: STRIDE, attack surface analysis
- Security headers: полный набор (`X-Content-Type-Options`, `X-Frame-Options`, `Strict-Transport-Security`, `Permissions-Policy`, `Cross-Origin-Embedder-Policy`, `Cross-Origin-Opener-Policy`)
- Secure session management: token rotation, session fixation prevention, idle timeout
- Cryptography на клиенте: Web Crypto API, hashing, encryption, key management
- Security audit: OWASP Top 10 для фронтенда, automated security scanning (SAST)
- Third-party dependency risk: lockfile integrity, supply chain security, SBOMs
- Trusted Types: предотвращение DOM-based XSS через Trusted Types API
- Sandboxing: iframe sandbox, Feature Policy, Permissions Policy
- Privacy: GDPR/CCPA compliance на фронтенде, consent management, data minimization

### Инфраструктура и DevOps

- Kubernetes: понимание deployment, service, ingress, HPA, health checks для фронтенд-сервисов
- CI/CD: проектирование пайплайнов для монорепозитория, affected detection, parallel execution, caching strategies
- Infrastructure as Code: Terraform/Pulumi для фронтенд-инфраструктуры (CDN, S3, CloudFront)
- Observability: structured logging, distributed tracing (OpenTelemetry), custom metrics, dashboards
- Sentry: performance monitoring, session replay, custom instrumentation, alerting rules
- Blue-green и canary deployments для фронтенда
- Feature flags: LaunchDarkly, Unleash — архитектура, SDK-интеграция, gradual rollouts
- A/B тестирование: архитектура, интеграция с analytics, statistical significance
- CDN: multi-region, cache invalidation strategies, edge functions
- SSL/TLS: certificate management, OCSP stapling, certificate pinning
- DNS: record types, TTL, failover, GeoDNS
- Preview deployments: per-PR environments, infrastructure automation
- Monitoring и alerting: SLI/SLO/SLA для фронтенда, error budgets

### Node.js (для фронтенд-инженера)

- Event Loop в Node.js: отличия от браузерного, libuv, фазы
- Streams: Readable, Writable, Transform, Duplex — piping, backpressure
- Кластеризация: `cluster` module, PM2, worker threads
- Express/Fastify: middleware, routing, error handling
- SSR-серверы: настройка, streaming, caching, hydration
- BFF: проектирование, API aggregation, data transformation, authentication proxy
- Файловая система: `fs` module, streaming reads/writes
- Environment management: `dotenv`, config management, secrets
- Diagnostics: Node.js inspector, CPU profiling, heap dumps, `--inspect`
- npm registry: публикация пакетов, scoped packages, provenance

### Браузерные технологии

- PWA: Web App Manifest, Service Workers (lifecycle, strategies), Background Sync, Push Notifications, installability
- WebAssembly: использование WASM-модулей на фронтенде, interop с JS, performance cases
- WebRTC: peer-to-peer, MediaStream, RTCPeerConnection, signaling
- Web Animations API: `element.animate()`, `getAnimations()`, timeline-based
- Navigation API: новый navigation management
- Speculation Rules API: prerendering
- Storage API: Storage Manager, persistence, quota estimation
- Credential Management API, Web Authentication API (WebAuthn, Passkeys)
- Payment Request API (знание что существует и когда применять)
- Screen Wake Lock API, Screen Orientation API
- Permissions API: запрос и проверка разрешений
- Shared Storage API и Private State Tokens (Privacy Sandbox)

### Работа с данными

- Алгоритмы поиска и сортировки: сложность, применение
- Big O notation: оценка сложности своего кода, оптимизация
- Нормализация данных: когда и зачем, denormalization trade-offs
- Кэширование данных на клиенте: стратегии, инвалидация, stale-while-revalidate
- Paginated data: infinite scroll, cursor-based pagination, virtual scrolling
- Offline data sync: conflict resolution strategies, last-write-wins, operational transforms
- Работа с большими данными на клиенте: Web Workers для вычислений, streaming processing

### Менеджмент и лидерство

- Определение и ведение технической стратегии команды
- Менторство: индивидуальные планы развития для членов команды
- Техническое собеседование: проектирование секций, калибровка, принятие решений о найме
- Code review culture: формирование практик, стандарты, автоматизация
- Управление техническим долгом: инвентаризация, приоритизация, защита ресурсов на рефакторинг
- Инцидент-менеджмент: post-mortem, root cause analysis, prevention measures
- Планирование: декомпозиция проектов на спринты, оценка рисков, buffer management
- Кросс-командное взаимодействие: синхронизация с бэкенд-командой, дизайном, продуктом, QA
- Техническая документация: RFC, ADR, runbook, onboarding guide
- Выступления: технические доклады внутри компании, knowledge sharing sessions
- Работа со стейкхолдерами: перевод технических решений на бизнес-язык
- Владение результатом, а не задачами: end-to-end delivery
- Проактивное выявление проблем и возможностей
- Адаптация к неопределённости и меняющимся приоритетам

### Soft skills

- Ведёт проекты от идеи до продакшена
- Самостоятельно находит решения для неоднозначных проблем
- Менторит Middle-разработчиков, выстраивает программу развития
- Проводит архитектурные ревью и принимает финальные решения
- Управляет ожиданиями стейкхолдеров
- Балансирует технический долг и бизнес-задачи
- Формирует инженерную культуру в команде
- Оценивает сроки проектов и управляет рисками
- Принимает решения в условиях неопределённости с обоснованием trade-offs
- Разрешает технические разногласия в команде
- Выстраивает эффективную коммуникацию с кросс-функциональными командами
- Думает об успехе команды и продукта, а не только о своих задачах
