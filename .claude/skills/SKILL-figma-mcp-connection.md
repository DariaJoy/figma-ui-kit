---
name: figma-mcp-connection
description: Use when establishing connection to Figma via MCP, parsing Figma URLs to extract fileKey/nodeId, verifying MCP read/write capabilities, writing components to existing files via use_figma, or troubleshooting MCP issues. Provides canonical procedures based on real-world experience including 2026-05-20 write experiment.
---

# Skill: Figma MCP Connection (v2)

> Стандартизованная процедура подключения и работы с Figma через MCP.
> Обновлено 2026-05-20 после эксперимента с write через `use_figma`.

## Зачем этот skill

При работе с Figma через Claude Code регулярно возникают одни и те же ситуации: «MCP не подключён», «нужно достать fileKey из URL», «как создать компонент через MCP», «как проверить Variables». Этот skill держит проверенные процедуры в одном месте.

---

## 1. Подключение Figma MCP к Claude Code

### Минимальная установка

```bash
claude plugin install figma@claude-plugins-official
```

Проверка: `claude` → `/plugin` → должна быть figma, статус enabled.

### Подключение Figma Desktop MCP server

1. Открыть Figma Desktop.
2. Открыть любой Design-файл.
3. Dev Mode (тумблер вверху).
4. Правая панель → блок MCP server → включить.
5. Скопировать URL (`http://127.0.0.1:3845/mcp`).

```bash
claude mcp add --transport http figma-desktop http://127.0.0.1:3845/mcp
```

Проверка: `/mcp` в Claude Code → figma-desktop, Connected.

### Если /mcp показывает «No MCP servers configured»

Это **stale view**. Проверь `claude mcp list`. Если сервер есть — открой новую сессию Claude Code.

---

## 2. Парсинг Figma URL

```
https://www.figma.com/design/<fileKey>/<имя>?node-id=<a>-<b>&t=...
```

- **fileKey** — между `/design/` и следующим `/`
- **nodeId** — `node-id=639-229` → передавай как `639:229` (дефис → двоеточие)

```python
import re
url = "https://www.figma.com/design/mklugArIdryCDV59zx31u3/Практика?node-id=639-229"
file_key = re.search(r'/design/([^/]+)/', url).group(1)
node_id = re.search(r'node-id=([^&]+)', url).group(1).replace('-', ':')
```

---

## 3. Чтение метаданных через MCP

### Базовый вызов

```
mcp__plugin__figma__figma__get_metadata
  fileKey: <key>
  nodeId: <id>
```

### Если результат слишком большой

При ошибке «exceeds maximum allowed tokens» — результат сохранён в файл. Использовать Python с UTF-8, **не PowerShell**:

```bash
python -c "
import json
with open('путь/к/файлу.txt', 'r', encoding='utf-8') as f:
    data = json.load(f)
text = data[0]['text']
print(text[:3000])
"
```

### Почему PowerShell не подходит

Windows PowerShell использует Windows-1251 для русской локали → кириллица превращается в `???????????`. Если PowerShell неизбежен — `Get-Content -Encoding UTF8`.

---

## 4. Проверка Figma Variables

```
mcp__plugin__figma__figma__get_variable_defs
  fileKey: <key>
```

- `{}` — Variables НЕ созданы. Это критическая находка для ревизии DS.
- Не пустой объект — Variables есть.

**Не доверяй документации.** В Figma может быть «секция цветов» с hex, выглядящая как DS. Но если `get_variable_defs` вернул `{}` — это документация, не работающие токены.

**Правило ревизии:** первый шаг — `get_variable_defs`. Потом всё остальное.

---

## 5. Write-возможности — ОБНОВЛЕНО 2026-05-20

### Что РАБОТАЕТ через `use_figma`

`mcp__plugin__figma__figma__use_figma` — **полноценная write-операция**. Выполняет произвольный JavaScript через Figma Plugin API. Все возможности любого Figma-плагина.

**Подтверждено в эксперименте 2026-05-20** (создан Navigation/Mobile в файле Практика):

| Операция | Статус |
|---|---|
| Создание фреймов | ✅ работает |
| Создание Figma Components (`figma.createComponent()`) | ✅ работает |
| Auto-layout (horizontal/vertical, space-between, padding) | ✅ работает |
| Текстовые слои с шрифтом и размером | ✅ работает |
| Прямоугольники, линии (`createRectangle()`) | ✅ работает |
| Hex-цвета на заливках | ✅ работает |
| Группировка элементов | ✅ работает |
| Установка координат и размеров | ✅ работает |
| Иерархия (вложенность) | ✅ работает |

### Что РАБОТАЕТ С ОГОВОРКАМИ

| Операция | Оговорка |
|---|---|
| Figma Variables (`figma.variables.*`) | API доступно. В пустой коллекции — безопасно. В существующей DS с захардкоженными компонентами — рискованно, может сломать привязки. Безопаснее через Tokens Studio. |
| Шрифты | `figma.loadFontAsync()` работает, но только если шрифт **установлен локально или уже использован в файле**. Для нового шрифта — сначала установить или импортировать в файл вручную. |
| ComponentSet с variants | Возможно через API, но требует опыта работы с component properties и variant naming. Не пробовали в эксперименте. |
| Привязка к существующим Variables | Возможно через ID Variables, но требует сначала прочитать Variables через `get_variable_defs`. |

### Что НЕ работает / есть ограничения

| Что | Почему |
|---|---|
| `mcp__figma-desktop__*` (локальный сервер) | В сессии эксперимента 2026-05-20 не зарегистрировал ни одного инструмента. Возможно, требует отдельной конфигурации. Все write идёт через `plugin:figma:figma`. |
| `get_metadata` на всю страницу сразу | Ответ может быть ~400 КБ XML, не помещается в контекст. Работать с конкретными node ID. |
| Скрипт `use_figma` если файл закрыт | Файл **должен быть открыт в Figma Desktop** в момент вызова. Это не облачный REST API, это плагин. |
| Атомарность | Скрипт целиком — атомарная транзакция. Упал = ничего не создалось. Лучше дробить сложные операции на мини-скрипты с проверкой после каждого. |

### Другие инструменты записи

| Инструмент | Назначение |
|---|---|
| `mcp__plugin__figma__figma__create_new_file` | Создание нового пустого файла |
| `mcp__plugin__figma__figma__upload_assets` | Загрузка изображений/ассетов |
| `mcp__plugin__figma__figma__add_code_connect_map` | Code Connect (маппинг компонент ↔ код) |
| `mcp__plugin__figma__figma__generate_figma_design` | Генерация дизайна (отдельная задача) |

---

## 6. Когда что использовать — карта решений

| Задача | Инструмент | Почему |
|---|---|---|
| Создать простой компонент в существующем файле | `use_figma` | Работает, проверено |
| Создать сложный компонент с variants и properties | `use_figma` с поэтапными скриптами + ручная доводка | Variants через API сложно |
| Создать Variables в чистой DS | `use_figma` (`figma.variables.*`) | Безопасно в пустой коллекции |
| Создать Variables в существующей DS с привязками | **Tokens Studio for Figma** (плагин) | Безопаснее, не сломает существующее |
| Создать макет страницы из дизайн-системы | `use_figma` итерационно (компонент за компонентом) | Работает |
| Импортировать готовый HTML-макет | **html.to.design** (плагин) | Быстрее, чем создавать с нуля |
| Сгенерировать состояния компонента (hover/focus/etc) | **Variants** (плагин Figma) | Автоматизация состояний |

**Принцип:** MCP-запись — мощно, но иногда плагины Figma — безопаснее и быстрее. Под каждую задачу — свой инструмент.

---

## 7. Канонический workflow «Дарья + Claude + Figma»

```
Дарья: задача в чате Claude Code
   ↓
Claude (как агент-роль): инструкции из .claude/agents/
   ↓
Claude (если нужен Figma): mcp__plugin__figma__figma__*
   ↓
ВЕТКИ:
   ├── Простая запись → Claude через use_figma → готово
   ├── Сложная запись → Claude готовит JSON/HTML → Дарья через плагин Figma
   └── Чтение / ревизия → Claude через get_metadata/get_variable_defs
   ↓
Claude: повторный вызов MCP для проверки (через get_metadata по нужным node ID)
   ↓
Дарья: коммит в Git
```

---

## 8. Безопасность при write-операциях

### Перед каждой write-сессией

1. **Бэкап файла** — File → Save local copy (`.fig` на диск) + Duplicate в облаке.
2. **Файл открыт в Figma Desktop** — закрытый файл не работает.
3. **Понятен node ID родительского контейнера** — куда положим новые узлы.
4. **Если работаешь с Variables — сначала `get_variable_defs`** — увидеть существующее.

### После каждой write-операции

1. **Проверка через `get_metadata`** на node ID созданного объекта.
2. **Скриншот** (если поддерживается `await node.screenshot()`) — для верификации в файле.
3. **Запись в CHANGELOG** проекта — что создано, когда, каким скриптом.

### Если что-то пошло не так

- **Атомарность скрипта** = ничего не создалось (Figma откатывает).
- **Если создалось, но криво** — Ctrl+Z в Figma Desktop, или удалить созданное вручную.
- **В крайнем случае** — восстановление из бэкапа.

---

## 9. Lessons learned

> Уроки из реальной работы со skill'ом.

- **2026-05-18 —** Перезапуск секретаря в той же сессии не сделал реальные вызовы MCP.
  - **Почему:** контекст-окно хранит результаты прошлых tool calls.
  - **Правило:** для повторных запусков — НОВАЯ сессия Claude Code.

- **2026-05-18 —** PowerShell исказил русские символы при парсинге JSON из MCP.
  - **Почему:** Windows-1251 по умолчанию.
  - **Правило:** Python с `encoding='utf-8'`, не PowerShell.

- **2026-05-18 —** `get_metadata` для большого фрейма превысил лимит токенов (~400 KB JSON).
  - **Почему:** Figma-фреймы с большим количеством вложенностей отдают огромные XML.
  - **Правило:** при ошибке «exceeds maximum allowed tokens» — Python для парсинга, работать по конкретным node ID, не «всё подряд».

- **2026-05-18 —** Документация UI-kit заявляла 9 цветовых токенов, `get_variable_defs` вернул `{}`.
  - **Почему:** дизайнеры часто описывают «токены» как визуальный список без создания Variables.
  - **Правило:** при ревизии DS первый шаг — `get_variable_defs`.

- **2026-05-20 — КРИТИЧЕСКИЙ УРОК** ⚠️: `use_figma` **работает на запись** в существующий Figma-файл. Был успешно создан Figma Component Navigation/Mobile с auto-layout, шрифтом Unbounded Bold, hex-цветами, иерархией. Все предыдущие предположения о «read-only характере» Figma MCP оказались неверны.
  - **Почему:** инструмент `use_figma` выполняет произвольный JS через Plugin API. Полноценный плагин Figma по своим возможностям. Не был замечен при первоначальной оценке.
  - **Правило:** для простых компонентов в существующем файле — `use_figma`. Для сложных вариативных или для Variables в существующей DS — плагины Figma (Tokens Studio, html.to.design). Бэкап перед каждой write-сессией. Файл должен быть открыт в Figma Desktop. Скрипт атомарный — дробить на мини-операции.

- **2026-05-20 —** В сессии эксперимента сервер `figma-desktop` (локальный MCP по адресу `http://127.0.0.1:3845/mcp`) не зарегистрировал инструменты. Все операции прошли через `plugin:figma:figma` (облачный).
  - **Почему:** возможно, конфигурация требует доработки или сервер figma-desktop активен только для определённых типов операций.
  - **Правило:** для write-операций использовать `plugin:figma:figma`. Локальный `figma-desktop` — на чтение dev-mode разметки и Variables. Не блокировать работу, если только один из двух доступен.
