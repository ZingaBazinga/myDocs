---
tags: [middle, css, architecture, performance]
---

# CSS — уровень Middle

База: [[09-Конспект-Junior-база#CSS]]. Ниже — темы Middle из матрицы.

---

## Архитектура CSS: BEM

**Block__Element--Modifier** — снижает коллизии имён без Shadow DOM. Пример: `card`, `card__title`, `card__title--highlighted`.

Плюсы: предсказуемость в больших командах. Минусы: длинные классы; при смешении с utility-first (Tailwind) договоритесь о стандарте.

---

## CSS Modules

Локальные классы: `.button` компилируется в `.button_a3f2`. **`composes`** подмешивает другие классы модуля.

Интеграция с Vite/Webpack через `import styles from './x.module.css'`. Удобно с React: `className={styles.button}`.

---

## CSS-in-JS: styled-components, Emotion, Linaria

**Runtime** (styled-components, Emotion): стили в JS, темы, динамика; цена — bundle и runtime.

**Zero-runtime** (Linaria): извлекает CSS на этапе сборки.

Trade-offs: DX vs производительность; SSR-нюансы; лучше единый подход в проекте.

---

## Продвинутые анимации

**`@keyframes`** + `animation-fill-mode: forwards` — финальное состояние удерживается.

**`will-change: transform`** — подсказка композитору; **злоупотребление** создаёт лишние слои и память.

**Дешёвые для композитора:** `transform`, `opacity`. Избегай анимации `width`, `top`, `margin` для тяжёлых участков — вызывают layout/paint.

**`requestAnimationFrame`** — синхронизация с кадром; см. [[03-JavaScript-Middle#requestAnimationFrame в цикле событий]].

---

## CSS Container Queries

**`@container`** — стили от ширины **контейнера**, а не viewport.

```css
.card { container-type: inline-size; }
@container (min-width: 400px) { .title { font-size: 1.25rem; } }
```

Нужен `container-type` (часто `inline-size`) и опционально `container-name` для именованных контейнеров.

---

## Cascade Layers: `@layer`

Порядок слоёв задаётся явно: например `@layer reset, theme, components, utilities`. Внутри слоя специфичность считается как обычно, но **слой** старше порядка в файле. Упрощает переопределения без `!important`.

---

## Subgrid

`grid-template-columns: subgrid` позволяет вложенной сетке наследовать дорожки родителя — выравнивание карточек с разной вложенностью.

---

## Logical properties

`margin-inline`, `padding-block`, `inline-size`, `block-size` — привязка к направлению текста; проще **RTL** и мультиязычность чем left/right.

---

## Scroll Snap

`scroll-snap-type: y mandatory` на контейнере + `scroll-snap-align: start` на элементах — карусели, секции fullscreen на мобилках.

---

## `aspect-ratio`

`aspect-ratio: 16 / 9` резервирует место под медиа → меньше **CLS**. Комбинируй с `width: 100%`.

---

## Селектор `:has()`

«Родитель от потомка»: `.card:has(img) { … }`. Мощный, но тяжёлый при злоупотреблении — профилируй.

---

## Градиенты и `@supports`

**`conic-gradient`**, сложные **radial** — для UI-иллюстраций. **`@supports (display: grid)`** — progressive enhancement.

---

## Print: `@media print`

Скрывай навигацию, раскрывай контент, задаёй `color-adjust`, размеры страницы. Часто `display: none` для `.no-print`.

---

## CSS Nesting (нативный)

Вложенность как в препроцессорах; внимание к специфичности и порядку вложенных `@media`.

---

## View Transitions API (база)

`document.startViewTransition(() => updateDOM)` — плавные переходы между состояниями SPA. Поддержка не везде — progressive enhancement.

---

## Современный цвет: `color-mix()`, `oklch()`

**OKLCH** — перцептивно равномерное пространство; проще подбирать палитры и контраст. **`color-mix(in oklch, var(--a) 70%, white)`** — предсказуемые оттенки.

---

## Связанные заметки

- [[07-A11y-Производительность-Безопасность-Middle#Core Web Vitals]] — CLS и изображения
- [[01-HTML-Middle#Critical Rendering Path]] — порядок CSS/JS
