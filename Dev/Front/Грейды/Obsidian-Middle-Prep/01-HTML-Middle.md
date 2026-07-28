---
tags: [middle, html, seo, web-components]
---

# HTML — уровень Middle

Включает всё из Junior-базы ([[09-Конспект-Junior-база#HTML]]). Ниже — **дополнения Middle** из матрицы.

---

## Микроразметка: Schema.org, JSON-LD, Open Graph, Twitter Cards

**Вопрос:** Зачем микроразметка и как её подключают?

**Ответ:** Поисковики и соцсети «читают» страницу не как человек. **Schema.org** — словарь сущностей (Product, Article, Organization). **JSON-LD** — рекомендуемый формат: объект в `<script type="application/ld+json">`, не ломает DOM и проще поддерживать.

**Open Graph** (`og:title`, `og:image`, `og:url`, `og:type`) — превью в Slack/Telegram/Facebook. **Twitter Cards** (`twitter:card`, `twitter:image`) — аналог для X: если заполнить OG, часто хватает, но для Twitter иногда нужны отдельные теги.

Практика: синхронизируй `title`/`description` с OG; проверяй валидаторы (Google Rich Results Test, Facebook Sharing Debugger).

---

## SEO: canonical, hreflang, robots, sitemap

- **`link rel="canonical"`** — канонический URL дубликата (фильтры, UTM, HTTP/HTTPS). Сигнал поиску: «индексируй этот URL».
- **`hreflang`** — языковые/региональные версии (`en-us`, `ru-ru`); пары страниц должны ссылаться друг на друга.
- **`<meta name="robots" content="noindex,nofollow">`** — точечно закрыть от индекса (страницы авторизации, черновики).
- **`sitemap.xml`** — список URL + `lastmod`; отправляется в Search Console; не заменяет качественную внутреннюю перелинковку.

---

## Продвинутые формы: FormData, файлы, drag-and-drop

**FormData API** собирает пары ключ–значение из формы или собирается вручную; удобно с `fetch` и `multipart/form-data` для загрузки файлов.

**Загрузка файлов:** `<input type="file" multiple accept="image/*">` + в JS `input.files` — это `FileList`. На сервер часто шлют именно `FormData`.

**Drag and Drop API:** события `dragenter`, `dragover`, `drop`, `dragleave`; нужно `preventDefault` на `dragover`, иначе браузер откроет файл. Для UX показывай зону подсветки и ограничения размера/типа.

---

## Web Components: template, slot, Shadow DOM, Custom Elements

**Custom Elements** — `customElements.define('my-widget', class extends HTMLElement { connectedCallback()… })`. Жизненный цикл: `connectedCallback`, `disconnectedCallback`, `attributeChangedCallback`, `observedAttributes`.

**Shadow DOM** — инкапсуляция стилей и разметки; `:host`, `::slotted`. Стили снаружи не протекают внутрь (и наоборот — с оговорками по CSS variables).

**`<template>`** — неактивный фрагмент DOM; клонируют через `template.content.cloneNode(true)`.

**`<slot>`** — проекция контента из светлого DOM в тень: именованные слоты `slot="header"`.

Trade-off: отлично для виджетов и дизайн-систем; интеграция с React иногда требует обёрток и внимания к событиям.

---

## Content Security Policy (CSP) через meta

**CSP** ограничивает источники скриптов, стилей, картинок — главная защита от XSS.

Пример в `<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">`.

Ограничение: **не всё** можно надёжно задать только meta; заголовок HTTP предпочтительнее. Nonce/hash для `script-src` в meta могут быть неудобны — часто CSP задают на сервере.

---

## Атрибуты загрузки: loading, decoding, fetchpriority

- **`loading="lazy"`** — отложенная загрузка img/iframe ниже сгиба; не ленить LCP-изображение (hero).
- **`decoding="async"`** — декодирование не блокирует main thread сильнее синхронного.
- **`fetchpriority="high"`** — подсказка приоритета для LCP; `low` для некритичных.

---

## Элемент `<dialog>` и модальные окна

Нативный `<dialog>` даёт `showModal()`, `close()`, псевдоэлемент `::backdrop`, фокус и закрытие по Escape (в модальном режиме).

Плюсы: меньше кода, доступность проще, чем с нуля. Минусы: стилизация backdrop, вложенные диалоги, старые браузеры (полифилы).

Связь с [[07-A11y-Производительность-Безопасность-Middle#Доступные компоненты]] — фокус-трап и возврат фокуса.

---

## Critical Rendering Path (CRP) — контекст HTML

Браузер: байты HTML → токены → **DOM**. Встретил `<link rel="stylesheet">` — запрос CSS → **CSSOM**. JS без `async/defer` блокирует парсинг.

HTML влияет на CRP: лишние синхронные скрипты в `<head>`, огромные инлайн-блоки, блокирующие шрифты — ухудшают First Paint. Решения: `defer`, `preload` критического CSS, разумный порядок ресурсов.

---

## `<link rel="preload|prefetch|preconnect|dns-prefetch">`

- **preload** — высокий приоритет «нужно в этом навигационном контексте» (шрифты, hero-image, критический JS как модуль).
- **prefetch** — низкий приоритет «может понадобиться следующей странице».
- **preconnect** — заранее TCP/TLS к origin (API CDN).
- **dns-prefetch** — только DNS; легче, чем preconnect.

---

## Inert и управление фокусом

**`inert`** на элементе делает потомков недоступными для фокуса и событий — удобно, когда модалка открыта: «заглушить» страницу под ней.

Управление фокусом — см. [[07-A11y-Производительность-Безопасность-Middle#Управление фокусом]].

---

## Связанные заметки

- [[02-CSS-Middle]] — слои, контейнерные запросы
- [[07-A11y-Производительность-Безопасность-Middle#Critical Rendering Path]] — углублённо
