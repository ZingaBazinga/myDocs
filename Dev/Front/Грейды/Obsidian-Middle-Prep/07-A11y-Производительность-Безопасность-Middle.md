---
tags: [middle, a11y, performance, security]
---

# Доступность, производительность, безопасность — Middle

---

## Доступность (A11y)

### ARIA: roles, states, properties, landmarks

**Роли** сообщают семантику (`navigation`, `dialog`, `tablist`). **States/properties** — `aria-expanded`, `aria-selected`, `aria-invalid`.

**Landmarks** (`role="main"`, `nav`, `banner`, `contentinfo`) помогают скринридерам прыгать по странице.

Правило: **не лечить плохую разметку ARIA** — сначала нативные элементы (`button`, `nav`).

### aria-live

**`aria-live="polite"`** — объявит после текущей речи; **`assertive"`** — прерывает (критические ошибки). Регион должен быть в DOM до обновления контента.

### aria-expanded, aria-hidden, aria-describedby, aria-labelledby

- **`aria-expanded`** — раскрыто ли меню/аккордеон (синхронизируй с клавиатурой).
- **`aria-hidden="true"`** — убрать декоративное из дерева доступности; **не** на фокусируемых без альтернативы.
- **`aria-describedby`** — дополнительное описание (подсказки, ошибки).
- **`aria-labelledby`** — видимая подпись для региона/модалки.

### Управление фокусом {#Управление фокусом}

**Focus trap** в модалке — Tab циклит внутри; при закрытии **возврат фокуса** на элемент, открывший модалку.

**Skip links** — первая фокусируемая ссылка «перейти к контенту».

### Клавиатура

Все интерактивные элементы достижимы Tab/Shift+Tab; видимый `:focus-visible`; не полагайся только на hover.

### Screen readers

VoiceOver (macOS/iOS), NVDA (Windows). Проверяй порядок чтения, имена кнопок, живые регионы.

### Контраст WCAG

Уровни **AA/AAA** для текста и UI-компонентов; инструменты: axe, Lighthouse, контраст-пикеры.

### Reduced motion

**`prefers-reduced-motion: reduce`** — отключить неessential анимации или заменить на fade.

### forced-colors

Режим высокой контрастности Windows — проверяй границы и видимость без только цвета.

### Доступные компоненты

Модалки, табы, dropdown, автокомплит, date picker — бери паттерны **WAI-ARIA APG** или проверенные библиотеки (Radix, Headless UI).

---

## Производительность {#Производительность}

### Core Web Vitals

- **LCP** — скорость главного контента; картинки, шрифты, серверный ответ.
- **INP** (ранее FID) — отзывчивость на ввод; дроби долгих задач на main thread.
- **CLS** — стабильность вёрстки; размеры для медиа, не вставлять контент над пользователем без резерва.

Измерение: CrUX, полевые RUM, Lighthouse лабораторные.

### Lighthouse / DevTools Performance

Профиль **Performance**: flame chart, **Long Tasks**, Main thread busy.

**Network**: waterfall, приоритеты, throttling.

### Critical Rendering Path {#Critical Rendering Path}

Минимизировать блокирующие ресурсы; inline critical CSS опционально; `defer` для скриптов; предзагрузка LCP-изображения.

### Изображения

**WebP/AVIF**, **`srcset`/`sizes`**, **`picture`** для art direction, **`loading="lazy"`** для некритичных.

### Шрифты

**`font-display: swap`**, **preload** критичного woff2, сабсеты, variable fonts; избегать невидимого текста без стратегии.

### Оптимизация бандла

Code splitting, lazy routes, vendor chunks; следить за тяжёлыми зависимостями.

### Кэширование

Долгий **`Cache-Control`** для hashed assets; Service Worker для офлайн/PWA по политике.

### Prefetch/preload

Из [[01-HTML-Middle]] — модуль **`modulepreload`** для ESM чанков.

### requestIdleCallback / rAF

Отложенная некритичная работа vs синхронизация анимаций — см. [[03-JavaScript-Middle#Event Loop: глубже]].

### Виртуализация списков

**@tanstack/virtual** — рендер только видимых строк для больших таблиц.

### Debounce/throttle

События scroll/resize/input — см. [[03-JavaScript-Middle#Debounce и Throttle]].

### Утечки памяти

Heap snapshots: отсоединённые DOM-узлы, растущие массивы листенеров.

---

## Безопасность

### XSS

**Stored / Reflected / DOM-based** — инъекция скриптов через HTML/URL/JS APIs.

Защита: экранирование, **`textContent`**, шаблоны React по умолчанию экранируют; **`dangerouslySetInnerHTML`** только с **DOMPurify**.

### CSRF

Непрошеные действия от лица пользователя: **CSRF-токены**, **`SameSite` cookies**, проверка заголовков (`Origin`).

### Content Security Policy

Директивы **`script-src`**, **`style-src`**, **`img-src`**, **`connect-src`**; **`nonce`** или **`hash`** для инлайн-скриптов.

Связь с [[01-HTML-Middle#Content Security Policy (CSP) через meta]].

### CORS

Браузер блокирует ответ без заголовков **`Access-Control-Allow-Origin`** для cross-origin XHR/fetch; **preflight** для «непростых» запросов.

### Secure cookies

**`HttpOnly`** (не читается из JS), **`Secure`** (только HTTPS), **`SameSite`**, префикс **`__Host-`**.

### JWT

Структура **header.payload.signature**; хранение в **`httpOnly` cookie** часто безопаснее localStorage от XSS; **refresh** + ротация; короткий TTL access token.

### HTTPS / TLS

Шифрование канала; **HSTS** принуждает HTTPS.

### SRI

**`integrity`** на `<script src>` с CDN — защита от подмены файла.

### Open Redirect

Валидируй параметр `redirect` против whitelist.

### Внешние ссылки

**`rel="noopener noreferrer"`** для `target="_blank"` — см. [[03-JavaScript-Middle]] (табуазард).

---

## Связанные заметки

- [[08-Архитектура-DevOps-Soft-Middle]]
- [[05-React-State-Network-Middle]]
