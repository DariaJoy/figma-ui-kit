# Figma binding — Nfluence

Привязка проекта Nfluence к конкретному Figma-файлу. Заполняется при первой синхронизации и обновляется при крупных изменениях файла.

## Аккаунт и доступ

| Поле | Значение |
|---|---|
| Метод подключения | OAuth через Figma MCP |
| MCP-серверы | `figma-desktop` (локальный) + `plugin:figma:figma` (облачный) |
| OAuth status | подключено |
| Handle | Дарья (DariaJoy на GitHub) |

## Главный Figma-файл проекта

| Поле | Значение |
|---|---|
| Название файла | Практика |
| File key | `mklugArIdryCDV59zx31u3` |
| URL | https://www.figma.com/design/mklugArIdryCDV59zx31u3/Практика |
| Назначение | Учебный файл-полигон. Содержит бриф, персоны, UI-kit Nfluence — всё в одном файле. |

## Ключевые узлы в файле

| Page name (Figma) | Node ID | Назначение | Куда мапится |
|---|---|---|---|
| Первый проект | `639:229` | Бриф (стратегический отчёт, 67 слайдов) | `01-brief-review.md` (если будет создан) |
| Навигация и архитектура сайта | `883:127` | Персоны (2 шт.), User Stories, User Flows, USM, архитектура (13 разделов) | `02-personas-review.md` (если будет создан) |
| Фундамент UI-kit | `1084:1030` | UI-kit v1.0 — палитра, типографика, компоненты, графика | `04-ui-kit-review.md`, `05-ui-kit-inventory.md` |

## Состояние Figma Variables

**Статус:** Variables созданы через MCP `use_figma` — 2026-05-22.

| Collection | ID | Mode | Переменных | Что внутри |
|---|---|---|---|---|
| Color | `VariableCollectionId:1195:128` | Dark | 14 | 9 тёмных токенов + 5 тег-акцентов |
| Spacing | `VariableCollectionId:1198:128` | Default | 7 | xs(4)…3xl(64) на базе 8px |
| Radius | `VariableCollectionId:1199:128` | Default | 6 | none/s/m/l/xl/pill(56) |

**Text Styles:** 9 стилей уже существовали (Display/80, Heading/H1–H4, Body/L–S, Body/Caption, Label/XS — Unbounded+Onest).

**Следующий шаг:** привязка Variables к существующим компонентам (спросить у Дарьи перед запуском — компонентов ~15).

> Примечание: в файле также есть старая коллекция «Variable collection» (9 переменных, светлая палитра) от другого учебного проекта — не относится к Nfluence.

## Связанные файлы Figma

На данный момент проект полностью в одном файле «Практика». В будущем возможно отделение в:

| Файл | Назначение | File key | URL | Last sync |
|---|---|---|---|---|
| Практика (текущий) | всё в одном | `mklugArIdryCDV59zx31u3` | см. выше | 2026-05-18 |

При необходимости — выделить:
- **UI-kit Nfluence (production)** — отдельный файл с финализированной библиотекой
- **Макеты Nfluence** — отдельный файл с экранами

## История синхронизаций

Журнал крупных синхронизаций (полный re-scan Figma-файла).

| Дата | Что синхронизировано | Кем | Результат |
|---|---|---|---|
| 2026-05-18 | Initial scan — бриф, персоны, UI-kit | secretary v3 | 00-intake.md создан |
| 2026-05-18 | Глубокий разбор UI-kit | design-system-architect (review) | 04-ui-kit-review.md, обнаружено отсутствие Variables |
| 2026-05-22 | Создание Variables Collections | design-system-architect (create-from-extract) | Color(14) + Spacing(7) + Radius(6); Text Styles уже существовали |

## Примечания

- **Бэкап:** перед каждым экспериментом с write-операциями MCP в этот файл — обязательно сохранять локальную копию (`File → Save local copy`) и/или облачную копию (`Duplicate`).
- **Доступ:** файл личный (Дарья), не командный. Один пользователь, один OAuth.
- **Платный тариф Figma:** Pro (нужен для full MCP access, особенно для записи).
