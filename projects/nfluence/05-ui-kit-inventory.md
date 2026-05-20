# UI-kit Inventory — Nfluence

Плоский индекс всех элементов UI-kit Nfluence v1.0 с реальным статусом (на основе ревизии 2026-05-18). Главный справочник для `layout-designer` и `frontend-engineer`.

**Источник правды:** Figma-файл `mklugArIdryCDV59zx31u3`, node `1084:1030`.
**Последняя синхронизация:** 2026-05-18 (через `mcp__plugin__figma__figma__get_metadata`).

## Условные обозначения

- ✅ **Готово** — Figma Component собран, можно использовать
- 🟡 **Частично** — собран, но без всех состояний или мобильной версии
- 📋 **Только в спецификации** — описан в Section 03 UI-kit, но Figma Component не создан
- ❌ **Нет** — не упомянут, нужно создать
- ⚠️ **Блокер** — без этого нельзя двигаться к layout-designer

---

## Foundations (фундамент)

### Цвета (Figma Variables)

| Категория | Статус Variables | Документация | Last sync |
|---|---|---|---|
| Brand colors | ❌ Variables не созданы | ✅ Section 01 UI-kit | 2026-05-18 |
| Text colors | ❌ Variables не созданы | ✅ Section 01 UI-kit | 2026-05-18 |
| Background colors | ❌ Variables не созданы | ✅ Section 01 UI-kit | 2026-05-18 |
| Accent colors | ❌ Variables не созданы | ✅ Section 01 UI-kit | 2026-05-18 |
| Border colors | ❌ Variables не созданы | ✅ Section 01 UI-kit | 2026-05-18 |
| State colors (error, success) | ❌ Variables не созданы | ⚠️ только base, нет state | 2026-05-18 |

**Критическая находка:** `get_variable_defs` → `{}`. Variables не созданы.

### Палитра (документация)

| Токен | Hex | Назначение | Контраст с bg-primary (WebAIM) |
|---|---|---|---|
| `bg/primary` | `#070809` | Основной фон | базовый |
| `bg/surface` | `#111314` | Карточки | ≈ 17:1 ✅ |
| `bg/elevated` | `#1E2123` | Модалки | ≈ 14:1 ✅ |
| `text/primary` | `#FFFFFF` | Основной текст | 21:1 ✅ AAA |
| `text/secondary` | `#A8ABAE` | Вторичный текст | ≈ 7.2:1 ✅ AA |
| `text/disabled` | `#4F5254` | Отключённый текст | ≈ 2.9:1 ❌ ниже AA |
| `accent/lime` | `#C5EE5B` | CTA, важные акценты | ≈ 12.1:1 ✅ AAA |
| `accent/silver` | `#E6E6E6` | Вторичный акцент | ≈ 18.5:1 ✅ AAA |
| `border/default` | `#252729` | Обычные границы | ≈ 1.5:1 (для границ норма) |

> Точные значения контраста — через WebAIM Contrast Checker.

### Типографика

| Шрифт | Источник | Кириллица | Применение | Статус |
|---|---|---|---|---|
| Unbounded | Google Fonts / Fontshare | ⚠️ есть, требует проверки Bold/Black на Щ/Ъ/Ю/Я | Display и заголовки H1-H2 | 📋 |
| Onest | Google Fonts | ✅ нативная | Заголовки H3-H4, body, UI | 📋 |

**Text Styles в Figma:** не подтверждены через MCP (вероятно тоже hardcode).

### Шкала размеров

| Токен | Размер | Line-height | Применение |
|---|---|---|---|
| display | 80px | 88px | Hero |
| h1 | 56px | 64px | Главные заголовки |
| h2 | 40px | 48px | Заголовки секций |
| h3 | 28px | 36px | Подзаголовки |
| h4 | 22px | 28px | Карточки |
| body-l | 18px | 28px | Лид, акцентный текст |
| body-m | 16px | 24px | Основной текст |
| body-s | 14px | 20px | Вторичный текст |
| caption | 12px | 16px | Подписи |
| label | 11px | 14px | Метки |

### Spacing

Базовая единица 8px. Шкала: 4, 8, 12, 16, 24, 32, 48, 64, 80, 120 px. Документация: ✅. Variables: ❌.

### Сетка

12 колонок · 24px gutter · 80px внешние отступы · max-width 1440px (desktop).
Mobile/tablet брейкпоинты: ❌ не определены.

### Радиусы и тени

| Радиус | Значение | Применение |
|---|---|---|
| (не определены явно) | — | Кнопки используют 56px (pill) |

Теней нет (flat-дизайн).

---

## Компоненты — реальное состояние

### Навигация

| Компонент | Статус | Variants | States | Адаптация | Заметки |
|---|---|---|---|---|---|
| `Navigation/Desktop` | ✅ Готово | один вариант | — | desktop only | hardcode цвета |
| `Navigation/Mobile` | ⚠️ **Блокер** | — | — | — | только в списке Section 03 |
| `Hamburger Menu` | ⚠️ **Блокер** | — | — | — | только в списке Section 03 |
| `Breadcrumb` | 📋 | — | — | — | только в списке |
| `Footer Desktop` | 📋 | — | — | — | только в списке |
| `Footer Mobile` | 📋 | — | — | — | только в списке |

### Кнопки & Действия

| Компонент | Статус | Variants | States | Заметки |
|---|---|---|---|---|
| `Button/Primary` | 🟡 Частично | один размер | Default, Hover, Disabled | ❌ нет Focus, Active, Loading |
| `Button/Secondary` | 🟡 Частично | один размер | Default, Hover, Disabled | ❌ нет Focus, Active, Loading |
| `Button/Ghost` | 🟡 Частично | один размер | Default, Hover, Disabled | ❌ нет Focus, Active, Loading |
| `Button/Icon` | 📋 | — | — | только в списке |
| `Button/Link` | 📋 | — | — | только в списке |
| `CTA Block` | 📋 | — | — | только в списке |

### Формы

| Компонент | Статус | States | Заметки |
|---|---|---|---|
| `Input/Text` | 🟡 Частично | Default, Focus, Filled, Error | ❌ нет Disabled, Loading |
| `Input/Textarea` | 📋 | — | только в списке |
| `Dropdown/Select` | 📋 | — | только в списке |
| `Checkbox` | 🟡 Частично | Unchecked, Checked | ❌ нет Disabled, Indeterminate |
| `Radio Button` | 📋 | — | только в списке |
| `Form Label` | 📋 | — | только в списке |
| `Form Error State` | 📋 | — | только в списке |

### Карточки

| Компонент | Статус | Заметки |
|---|---|---|
| `Card/Project` | 🟡 Частично | ❌ нет Hover |
| `Card/Услуга` | 📋 | только в списке |
| `Card/Кейс` | 📋 | только в списке |
| `Card/Блогер` | 📋 | нужны поля статистики (охват, ER) |
| `Card/Команда` | 📋 | возможно лишний — раздела «Команда» в архитектуре нет |
| `Card/Пост блога` | 📋 | только в списке |

### Hero & Секции

| Компонент | Статус | Заметки |
|---|---|---|
| `Hero / Fullscreen Video` | 📋 | только в списке |
| `Hero / Text Overlay` | 📋 | только в списке |
| `Stats Block` | 🟡 Частично | есть в in-context (Section 08) как inline-блок, не как компонент |
| `Section Header` | 📋 | только в списке |
| `Quote / Testimonial` | 📋 | только в списке |

### Контент

| Компонент | Статус |
|---|---|
| `Tag / Badge` | 🟡 Частично (3 варианта в Section «Атомы») |
| `Chip / Pill` | 📋 |
| `Divider` | 📋 |
| `Accordion` | 📋 |
| `Tabs` | 📋 |
| `Tooltip` | 📋 |

### Медиа

| Компонент | Статус |
|---|---|
| `Image Frame` | 📋 |
| `Video Preview` | 📋 |
| `Gallery Grid` | 📋 |
| `Avatar` | ✅ Готово (Small 32, Medium 48, Large 64) |

### Фидбэк & Оверлеи

| Компонент | Статус |
|---|---|
| `Toast / Alert` | 📋 |
| `Modal Dialog` | 📋 |
| `Overlay / Drawer` | 📋 |
| `Loader / Spinner` | 📋 |

---

## Графические элементы

В Section 04 UI-kit определён графический сет из ~14 векторных элементов:
Персонаж 01, Персонаж 02, Блоб, Волна 01, Волна 02, Бабочка, Group, Emoji, Arrow, Plant, Headphones, Heart, Sparkle, Face.

Источник: вдохновение от e-b-agency.com. Все 300×300px. **Статус: ✅ Готовы как векторы**, но не Figma Components.

---

## Сводка для layout-designer

### Минимум для desktop-главной страницы (можно начинать сейчас)

- ✅ Navigation/Desktop
- 🟡 Button/Primary, Secondary (без Focus)
- 🟡 Stats Block (inline → можно вынести в компонент)
- 🟡 Card/Project
- Графические элементы (✅ Готовы)

### Блокеры для полного сайта (≈ 2.5 часа работы в Figma)

| Блокер | Приоритет | Оценка |
|---|---|---|
| Создать Figma Variables (Color + Typography + Spacing) | Высокий | 1 час (через Tokens Studio) |
| Navigation/Mobile + Hamburger | Критический | 30 мин |
| Role Switcher («Для брендов / Для блогеров») | Критический | 30 мин |
| Focus-состояние у кнопок (WCAG 2.4.7) | Критический | 15 мин |
| Stats Block как компонент | Высокий | 15 мин |
| Process/Timeline (шаги «Как работаем») | Высокий | 30 мин |
| Footer Desktop/Mobile | Высокий | 30 мин |

### Средний приоритет (после первых макетов)

- Hover у Card/Project
- Card/Блогер со статистикой
- Accordion (FAQ)
- Form/Blogger и Form/Brand (вариативные)
- Адаптация типографики для mobile (Display 80 → 40–48px)
- Анимационная спецификация (Motion + Scrollytelling)
- Проверка Unbounded Cyrillic в браузерах

---

## История изменений inventory

| Дата | Что | Кем |
|---|---|---|
| 2026-05-18 | Создан на основе `04-ui-kit-review.md` | design-system-architect (ручная сводка) |
