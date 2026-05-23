# TEST-REPORT: Pipeline Nfluence

**Дата закрытия:** 2026-05-22
**Цель теста:** проверить работу автоматической цепочки 5 агентов на реальном учебном проекте
**Вердикт: ПРОЙДЕН** — все 5 этапов отработали, артефакты на диске и в Figma

---

## 1. Что сделано: этапы 1–5

| # | Агент | Режим | Дата | Вход | Выход | Вердикт |
|---|---|---|---|---|---|---|
| 1 | `secretary` v3 | intake | 2026-05-18 | Описание клиента устно | `00-intake.md` + sanity-check 4.2 | ✅ |
| 2 | `brief-analyst` v3 | review | 2026-05-21 | Figma node `639:229` (67 слайдов) | `01-brief-review.md` | ✅ С ЗАМЕЧАНИЯМИ |
| 3 | `ux-researcher` v3 | review | 2026-05-21 | Figma node `883:127` (персоны + USM + IA) | `02-personas-review.md` | ✅ ДОСТАТОЧНО |
| 4 | `moodboard-curator` v3 | EXTRACT | 2026-05-21 | 3 URL (e-b-agency, truus.co, techto.org) | `03-moodboard-extracted.md` | ✅ |
| 5 | `design-system-architect` v4 | CREATE-FROM-EXTRACT | 2026-05-18 + 2026-05-22 | Мудборд + UI-kit review | Variables в Figma | ✅ |
| + | `design-system-architect` | rename | 2026-05-22 | Variables без префикса | Variables с `Nfl_` | ✅ |

**Примечание по этапам 2 и 3:** они запущены как pipeline rerun (2026-05-21), так как на 2026-05-18 пайплайн работал в другом порядке (сначала moodboard-review, потом ui-kit-review). Финальный прогон выровнял цепочку.

**Этапы до pipeline-теста (2026-05-18):**
- `moodboard-curator` review → `03-moodboard-review.md` (Вариант A принят)
- `design-system-architect` review → `04-ui-kit-review.md` + `05-ui-kit-inventory.md`
- Эксперимент use_figma → создан Navigation/Mobile (доказательство концепции write через MCP)

---

## 2. Артефакты

### Файлы на диске

| Файл | Агент | Создан | Статус |
|---|---|---|---|
| `00-intake.md` | secretary v3 | 2026-05-18 | финальный |
| `01-brief-review.md` | brief-analyst v3 | 2026-05-21 | финальный |
| `02-personas-review.md` | ux-researcher v3 | 2026-05-21 | финальный |
| `03-moodboard-review.md` | moodboard-curator v3 | 2026-05-18 | финальный (режим review) |
| `03-moodboard-extracted.md` | moodboard-curator v3 | 2026-05-21 | финальный (режим EXTRACT) |
| `04-ui-kit-review.md` | design-system-architect v4 | 2026-05-18 | финальный |
| `05-ui-kit-inventory.md` | design-system-architect v4 | 2026-05-18 | черновик (не всё задокументировано) |
| `figma-binding.md` | secretary + DS-arch | 2026-05-18, обновлён 2026-05-22 | актуальный |
| `CLAUDE-nfluence.md` | secretary | 2026-05-18 | актуальный |
| `CHANGELOG-nfluence.md` | secretary | 2026-05-18 | актуальный |

**Не создан:** `04-ui-kit-spec.md` — по плану pipeline должен был быть выход design-system-architect CREATE-FROM-EXTRACT, но создание Variables через use_figma заменило текстовую спецификацию. Стоит создать как сводный документ.

### Variables в Figma (file key: `mklugArIdryCDV59zx31u3`)

| Collection | ID | Mode | Кол-во | Переменные |
|---|---|---|---|---|
| Color | `VariableCollectionId:1195:128` | Dark | 14 | `Nfl_bg/primary #070809`, `Nfl_bg/surface #111314`, `Nfl_bg/elevated #1e2123`, `Nfl_text/primary #ffffff`, `Nfl_text/secondary #a8abae`, `Nfl_text/disabled #4f5254`, `Nfl_accent/lime #c5ee5b`, `Nfl_accent/silver #e6e6e6`, `Nfl_border/default #252729` + 5 тег-акцентов |
| Spacing | `VariableCollectionId:1198:128` | Default | 7 | `Nfl_xs`(4px), `Nfl_s`(8px), `Nfl_m`(16px), `Nfl_l`(24px), `Nfl_xl`(32px), `Nfl_2xl`(48px), `Nfl_3xl`(64px) |
| Radius | `VariableCollectionId:1199:128` | Default | 6 | `Nfl_none`(0), `Nfl_s`(4px), `Nfl_m`(8px), `Nfl_l`(12px), `Nfl_xl`(16px), `Nfl_pill`(56px) |

**Text Styles (существовали до теста, не создавались):** Display/80, Heading/H1–H4, Body/L–S, Body/Caption, Label/XS — 9 стилей, Unbounded+Onest.

**Итого Variables:** 27 переменных в 3 коллекциях.

---

## 3. Оценка токенов по этапам

> Оценка приблизительная. Токены — это Claude context (prompt + completion), не API-токены Figma.

| Этап | Агент + режим | Оценка токенов | Основная нагрузка |
|---|---|---|---|
| Intake (secretary) | intake | ~20–30K | Чтение Figma MCP (бриф+персоны+UI-kit), написание 00-intake.md |
| Бриф ревизия | brief-analyst review | ~35–50K | Чтение 67 слайдов через Python UTF-8, анализ 6 разделов |
| Персоны ревизия | ux-researcher review | ~25–40K | Чтение Figma node (персоны+USM+IA), сверка с брифом и UI-kit |
| Мудборд EXTRACT | moodboard-curator EXTRACT | ~50–80K | 3× web_fetch + curl CSS-файлов + 03-moodboard-extracted.md |
| UI-kit review | DS-architect review | ~30–50K | MCP-сканирование UI-kit + составление 04+05 |
| MCP-эксперимент | DS-architect | ~20–30K | use_figma Navigation/Mobile (итерации с ошибками) |
| Variables CREATE | DS-architect CREATE | ~25–40K | 3 коллекции × use_figma + верификация + git |
| Переименование | DS-architect | ~5–10K | Rename скрипт + верификация |
| **Итого (оценка)** | | **~210–330K** | |

**Наблюдение:** самый токен-дорогой этап — moodboard EXTRACT из-за объёма web_fetch (HTML + CSS внешних сайтов). Второй по дороговизне — brief-analyst из-за 67 слайдов.

---

## 4. Что узнали нового: lessons learned

### Архитектура системы

**L1 — use_figma умеет писать в существующий файл.** Это критическое открытие 2026-05-20. Все предыдущие предположения о read-only Figma MCP были неверны. Теперь design-system-architect создаёт Variables и компоненты напрямую, без Tokens Studio.

**L2 — get_variable_defs работает по-узловому, не глобально.** Вызов с node-id возвращает переменные, привязанные к конкретному узлу, а не глобальный список. Для инспекции коллекций нужен `figma.variables.getLocalVariableCollectionsAsync()` через use_figma.

**L3 — Text Styles нужно проверять перед созданием.** При попытке создать Text Styles выяснилось, что 9 стилей уже существовали. Inspector-first подход сэкономил ~10K токенов и предотвратил дубликаты.

**L4 — Context compaction обрывает сессию mid-task.** Длинные сессии (Variables + компоненты в одном прогоне) рискуют потерять контекст при сжатии. Правило: один артефакт = одна сессия, коммит после каждого.

**L5 — Префиксы переменных нужны с самого начала.** Файл «Практика» содержит переменные от нескольких учебных проектов. Пришлось переименовывать 27 переменных постфактум. Правило: при создании Variables сразу использовать `<Slug>_` prefix.

### Работа агентов

**L6 — secretary правильно делает sanity-check на согласованность документов.** Находка «зрелый бриф vs дерзкий UI-kit» (4.2) дала критический инсайт, которого не было бы при простом intake. Это лучшая часть secretary v3.

**L7 — ux-researcher review ценнее create для проектов с готовыми материалами.** Персоны из брифа уже были полноценными. Reviewer добавил тест «персоны конфликтуют», привязку к UI-kit, список критических пробелов — то, что create из нуля не дал бы без реального Figma-файла.

**L8 — moodboard EXTRACT из Tilda-сайтов ограничен.** e-b-agency.com на Tilda хранит стили в dynamically-генерируемых блоках, прямой CSS не добывается. Лимоновый акцент извлечён визуально, не из CSS. Для Tilda-сайтов нужен скриншот + пипетка, а не web_fetch.

**L9 — Цепочка допускает параллельные ветки.** brief-analyst и ux-researcher могут работать параллельно, если оба читают из Figma. Сейчас они запускаются последовательно — это медленнее, чем могло быть.

---

## 5. Что НЕ сделано из плана и почему

| Пункт плана | Статус | Причина |
|---|---|---|
| `04-ui-kit-spec.md` (текстовая спецификация) | ❌ не создан | Variables в Figma заменили текстовый документ. Технически лучше, документально — пробел |
| Привязка Variables к существующим компонентам | ❌ не сделано | Протокол: «перед привязкой — отдельный вопрос». Рискованная операция, ~15 компонентов с hardcode |
| `Role Switcher` («Для брендов / Для блогеров») | ❌ не создан | Самый сложный компонент (интерактивный tab). Выходит за рамки Variables-теста |
| `Navigation/Mobile` как полноценный компонент | ⚠️ создан частично | Создан в MCP-эксперименте 2026-05-20, но не дооформлен как production-компонент |
| `Process/Timeline`, `Stats Block`, `Footer` | ❌ не созданы | За рамками Pipeline-теста. Блокеры для layout-designer |
| `Focus`-состояния кнопок (WCAG 2.4.7) | ❌ не добавлены | За рамками Variables-этапа |
| Светлая ветка Variables (#F5F4F0, #0F1A2E) | ❌ отложена | Решение Дарьи 2026-05-22: добавить после первых макетов тёмной ветки |
| Lora Italic как Text Style | ❌ не создан | Решение 2026-05-22: Lora применяется только для 1–2 слов в Hero, не нужен отдельный Text Style — применять напрямую |

---

## 6. Рекомендации на будущее

### Изменения в агентах

**R1 — secretary: добавить шаг «Variables audit».**
При MCP-сканировании UI-kit явно проверять: (a) есть ли Variables коллекции, (b) есть ли Text Styles, (c) привязаны ли переменные к компонентам. Сейчас это находится в ревизии DS-architect, но стоит выявлять уже на intake.

**R2 — design-system-architect: добавить режим BIND.**
Сейчас агент умеет CREATE и REVIEW. Нужен режим `BIND` — привязка существующих Variables к существующим компонентам. Это отдельный, рискованный режим с обязательным pre-backup.

**R3 — moodboard-curator: добавить скриншот-путь для Tilda/JS-сайтов.**
Когда web_fetch не даёт CSS (Tilda, Webflow с JS-only стилями) — переключаться на `get_screenshot` + визуальный анализ цветов, явно помечая их как `[наблюдение]`. Сейчас граница факт/предположение нарушается без предупреждения.

**R4 — добавить Variables в список проверок secretary.**
Если Variables отсутствуют — явно сигнализировать: «UI-kit без токенов → дополнительный этап до layout-designer». Сейчас это выявляется только в ревизии DS-architect.

### Архитектурные изменения

**R5 — Машиночитаемый статус pipeline.**
Сейчас статус цепочки — это текст в CLAUDE.md проекта. Нужен структурированный `pipeline-state.json` или таблица в CLAUDE.md с enum-статусами (`pending`, `in_progress`, `done`, `skipped`), которую каждый агент читает и обновляет. Это предотвратит rerun «с начала» при смене сессии.

**R6 — Параллельный запуск brief-analyst + ux-researcher.**
Оба читают из Figma независимо. При запуске через `Agent` tool с `run_in_background` можно сократить время pipeline на ~40%.

**R7 — Preflight-check перед layout-designer.**
Создать мини-агент или чеклист, который перед стартом layout-designer автоматически проверяет: Variables привязаны, Navigation/Mobile есть, Role Switcher есть, Focus-состояния есть. Без этого layout-designer начнёт на неполном фундаменте.

**R8 — Naming convention Variables с самого создания.**
Правило: Variables создаются с `<Slug>_` prefix сразу. Переименование 27 переменных постфактум — лишняя операция.

---

## 7. Итоговая оценка готовности к layout-designer

| Блокер | Статус |
|---|---|
| Variables (Color/Spacing/Radius) | ✅ готово |
| Text Styles | ✅ готово |
| Navigation/Desktop | ✅ готово |
| Navigation/Mobile | ⚠️ MVP-заготовка, требует доработки |
| Role Switcher | ❌ не создан |
| Process/Timeline | ❌ не создан |
| Stats Block | ❌ не создан |
| Form × 2 (Бренд / Блогер) | ❌ не создан |
| Variables привязаны к компонентам | ❌ hardcode |

**Вывод:** для desktop-лендинга (только Hero + Nav + Cards) — можно начинать. Для полного сайта по архитектуре 13 разделов — нужны ещё 4–5 компонентов плюс привязка Variables.

---

## 8. Журнал теста

| Дата | Событие |
|---|---|
| 2026-05-18 | Старт. Intake, moodboard-review, ui-kit-review, MCP-эксперимент |
| 2026-05-20 | Открытие: use_figma умеет писать в Figma. CHANGELOG обновлён |
| 2026-05-21 | Pipeline rerun: brief-analyst + ux-researcher + moodboard EXTRACT |
| 2026-05-22 | Variables CREATE (Color/Spacing/Radius), переименование Nfl_, закрытие теста |
