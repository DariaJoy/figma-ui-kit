# Changelog — Nfluence

Журнал крупных изменений в проекте Nfluence.

---

## 2026-05-20

- **Эксперимент с MCP-записью в Figma — УСПЕХ.** Через `mcp__plugin__figma__figma__use_figma` создан компонент `Navigation/Mobile` в существующем файле «Практика» (по координатам x=-9078, y=62000):
  - Родительский фрейм «Mobile Components — MCP Experiment» (439×128px)
  - Figma Component `Navigation/Mobile` (375×64px) с horizontal auto-layout, space-between, padding 16px
  - Текстовый слой «Nfluence» — Unbounded Bold 18px, цвет `#C5EE5B`
  - Иконка Hamburger — 3 прямоугольника 24×2px с vertical auto-layout, gap 6px, цвет `#FFFFFF`
  - Фон компонента: `#070809`
  - Подтверждено через `get_metadata`
- **КРИТИЧЕСКИЙ ПЕРЕСМОТР ПРАВИЛ.** Предыдущие предположения о «read-only характере» Figma MCP — неверны. См. урок 2026-05-20 в корневом CLAUDE.md.
- **Обновлены документы:**
  - Корневой `CLAUDE.md` — добавлен урок 2026-05-20 о write через `use_figma`
  - `.claude/skills/figma-mcp-connection/SKILL.md` — обновлены разделы 5 (Write-возможности) и 6 (Карта решений)
  - `.claude/agents/design-system-architect.md` — v4, добавлены Опции A/B для создания компонентов

---

## 2026-05-18

- **Создан `00-intake.md`** через secretary v3. Подтверждены и оценены через Figma MCP: бриф (67 слайдов), персоны (2 шт.), UI-kit «Фундамент v1.0».
- **Sanity-check выявил расхождение** «зрелый бриф vs дерзкий UI-kit». Зафиксировано как ключевой стратегический вопрос.
- **Создан `03-moodboard-review.md`** через moodboard-curator v3 в режиме review. Вердикт: «С замечаниями». Три критических находки: отсутствие мобильных компонентов, нерешённое расхождение характера, нет анимационной спецификации.
- **Принято Решение 1: Вариант A** по характеру (визуал не меняем, «зрелость» через контент).
- **Обновлён moodboard-curator до v3** с эпистемологическими правилами по итогам собственной ревизии.
- **Создан `04-ui-kit-review.md`** через design-system-architect (review). Вердикт: «С замечаниями». Главная находка: `get_variable_defs` → `{}`, Figma Variables не созданы. 24 из 39 заявленных компонентов существуют только как текстовый список.
- **Изучен внешний шаблон meta-pp**. Решено впитать: Lessons learned в каждом агенте, иерархические CLAUDE.md, figma-binding.md, inventory.md.
- **Создан `05-ui-kit-inventory.md`** — реальный индекс компонентов с привязкой к Figma node ID и статусом каждого.
- **Создан `figma-binding.md`** — явная привязка проекта к Figma-файлу.
- **Создан `CLAUDE.md` проекта** с состоянием по цепочке, ключевыми решениями, особенностями.
- **Все 5 агентов обновлены до v3 с Lessons learned**.

---

## Что дальше

- Применить открытие 2026-05-20: создать через `use_figma` остальные блокеры UI-kit:
  - [x] `Navigation/Mobile` — создан в эксперименте
  - [ ] `Hamburger Menu` — частично создан в составе Navigation/Mobile, можно вынести в отдельный компонент
  - [ ] `Role Switcher` («Для брендов / Для блогеров»)
  - [ ] `Focus`-состояния у кнопок (WCAG 2.4.7)
  - [ ] `Stats Block` как компонент
  - [ ] `Process/Timeline` (шаги «Как мы работаем»)
  - [ ] `Footer Desktop/Mobile`
- Для **Figma Variables** — отдельная решение (use_figma vs Tokens Studio). Безопаснее Tokens Studio.
- Создать агента `layout-designer`.
- Запустить layout-designer на desktop-главную страницу.
