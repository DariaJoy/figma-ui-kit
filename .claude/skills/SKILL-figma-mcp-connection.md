---
name: figma-mcp-connection
description: Use when establishing connection to Figma via MCP, parsing Figma URLs to extract fileKey/nodeId, verifying MCP read/write capabilities, or troubleshooting "no MCP servers configured" errors. Provides canonical procedures based on real-world experience.
---

# Skill: Figma MCP Connection

> Стандартизованная процедура подключения и работы с Figma через MCP. Создана по результатам работы на проекте Nfluence.

## Зачем этот skill

При работе с Figma через Claude Code регулярно возникают одни и те же ситуации: «MCP не подключён», «нужно достать fileKey из URL», «как проверить, что Variables существуют». Этот skill держит проверенные процедуры в одном месте.

---

## 1. Подключение Figma MCP к Claude Code

### Минимальная установка

```bash
# Проверить, что есть Figma plugin для Claude Code
claude plugin install figma@claude-plugins-official

# Проверить
claude
> /plugin
# должна быть figma со статусом enabled
```

### Подключение Figma Desktop MCP server

1. Открыть Figma Desktop (не браузерную).
2. Открыть любой Design-файл.
3. Переключиться в Dev Mode (тумблер вверху).
4. В правой панели найти блок MCP server.
5. Включить переключатель «Enable MCP server».
6. Скопировать URL (обычно `http://127.0.0.1:3845/mcp`).

```bash
# Добавить сервер в Claude Code
claude mcp add --transport http figma-desktop http://127.0.0.1:3845/mcp

# Проверить
claude
> /mcp
# должен быть figma-desktop, статус Connected
```

### Если /mcp показывает «No MCP servers configured»

Это может быть **stale view**. Проверка:

```bash
claude mcp list
```

Если в `mcp list` сервер есть, а в `/mcp` его нет — открой новую сессию Claude Code.

---

## 2. Парсинг Figma URL

URL Figma всегда имеет структуру:
```
https://www.figma.com/design/<fileKey>/<имя>?node-id=<a>-<b>&t=...
```

| Поле | Где взять | Пример |
|---|---|---|
| fileKey | между `/design/` и следующим `/` | `mklugArIdryCDV59zx31u3` |
| nodeId (для MCP) | значение `node-id`, дефис → двоеточие | `639-229` → `639:229` |

### Скрипт извлечения (Python)

```python
import re

url = "https://www.figma.com/design/mklugArIdryCDV59zx31u3/Практика?node-id=639-229&t=..."

file_key_match = re.search(r'/design/([^/]+)/', url)
file_key = file_key_match.group(1) if file_key_match else None

node_id_match = re.search(r'node-id=([^&]+)', url)
node_id = node_id_match.group(1).replace('-', ':') if node_id_match else None

print(f"fileKey: {file_key}")
print(f"nodeId (для MCP): {node_id}")
```

---

## 3. Чтение метаданных через MCP

### Базовый вызов

```
mcp__plugin__figma__figma__get_metadata
  fileKey: mklugArIdryCDV59zx31u3
  nodeId: 639:229
```

### Если результат слишком большой

Figma MCP возвращает ошибку:
> Error: result (66 587 characters) exceeds maximum allowed tokens. Output has been saved to ...

**Решение:** результат уже сохранён в файл. Не пытайся читать его построчно (Read с line-based offset не справится с большим JSON).

### Обработка большого JSON через Python

**Важно:** использовать UTF-8 явно, иначе кириллица превратится в `???????????`.

```bash
python -c "
import json
with open('путь/к/файлу.txt', 'r', encoding='utf-8') as f:
    data = json.load(f)
text = data[0]['text']
print(text[:3000])
"
```

Для поиска по содержимому:
```bash
python -c "
import json, re
with open('путь', 'r', encoding='utf-8') as f:
    data = json.load(f)
text = data[0]['text']
# Найти все имена фреймов
names = re.findall(r'name=\"([^\"]+)\"', text)
print(set(names))
"
```

### Почему PowerShell не подходит

Windows PowerShell по умолчанию использует Windows-1251 для русской локали. При выводе русского текста получаются `??????`. Если необходимо использовать PowerShell — обязательно `-Encoding UTF8`:

```powershell
$content = Get-Content -Path "путь" -Encoding UTF8 -Raw
```

---

## 4. Проверка Figma Variables

```
mcp__plugin__figma__figma__get_variable_defs
  fileKey: <fileKey>
```

### Интерпретация ответа

- `{}` — Variables **не созданы** в файле. Все значения захардкожены в компонентах. Это критическая находка для ревизии.
- Не пустой объект — Variables есть. Структура: `{ collection_id: { variable_id: { name, value, ... } } }`.

### Не доверяй документации

В Figma может быть «секция цветов» с hex-значениями, выглядящая как дизайн-система. Но если `get_variable_defs` вернул `{}` — это **документация на бумаге**, не работающие токены.

**Правило ревизии:** первый шаг — `get_variable_defs`. Только потом всё остальное.

---

## 5. Write-возможности — что НЕ работает

На текущей версии Figma MCP (май 2026):

| Операция | figma-desktop | plugin:figma:figma |
|---|---|---|
| Чтение метаданных | ✅ | ✅ |
| Чтение Variables | ✅ | ✅ |
| Чтение dev-mode (CSS, dimensions) | ✅ | ✅ |
| Создание Variables | ❌ | ⚠️ нестабильно |
| Создание простых фреймов | ❌ | ⚠️ через `generate_figma_design` — в новый файл |
| Создание компонентов с auto-layout | ❌ | ❌ |
| Создание variants | ❌ | ❌ |
| Привязка к Variables | ❌ | ❌ |
| Адаптивные варианты mobile/tablet/desktop | ❌ | ❌ |

**Вывод:** для production-работы Figma MCP — это инструмент **чтения**. Запись делает Дарья руками или через специальные плагины Figma (Tokens Studio, html.to.design, Variants).

---

## 6. Рекомендуемые плагины Figma для записи

Эти плагины **компенсируют** ограничения MCP, принимая на вход подготовленные Claude данные:

| Плагин | Назначение | Вход от Claude |
|---|---|---|
| **Tokens Studio for Figma** | Создаёт Variables из JSON | JSON в формате Tokens Studio |
| **html.to.design** | Превращает HTML/CSS в редактируемые слои Figma | HTML-разметка экрана/компонента |
| **Variants** | Генерирует состояния компонента | Список состояний |
| **Content Reel** | Заполняет тексты реалистичным контентом | (не нужен ввод от Claude) |

---

## 7. Канонический workflow «Дарья + Claude + Figma»

```
Дарья: задача в чате Claude Code
   ↓
Claude (как агент-роль): инструкции из .claude/agents/
   ↓
Claude (если нужен Figma): mcp__plugin__figma__figma__*
   ↓
Claude (если нужно создавать в Figma): JSON / HTML / спецификация
   ↓
Дарья: открывает плагин в Figma, импортирует
   ↓
Figma: реальные Variables / компоненты появляются
   ↓
Claude: повторно вызывает MCP для проверки
   ↓
Дарья: коммит в Git
```

---

## 8. Lessons learned

> Уроки из реальной работы со skill'ом. Обновляется после каждой существенной находки.

- **2026-05-18 —** Перезапуск секретаря в той же сессии не сделал реальные вызовы MCP, переиспользовав данные предыдущей сессии.
  - **Почему:** контекст-окно сессии хранит результаты прошлых tool calls, модель оптимизирует токены.
  - **Правило:** для повторного запуска агента — НОВАЯ сессия Claude Code. Удалить или переименовать предыдущий артефакт.

- **2026-05-18 —** PowerShell исказил русские символы при парсинге JSON-результата MCP (получились `???????????`).
  - **Почему:** Windows PowerShell использует Windows-1251 для русской локали по умолчанию.
  - **Правило:** для работы с русским текстом — Python с `encoding='utf-8'`, не PowerShell. Если PowerShell неизбежен — `-Encoding UTF8`.

- **2026-05-18 —** `get_metadata` для большого фрейма (67 слайдов брифа) превысил лимит токенов.
  - **Почему:** Figma-фреймы с большим количеством вложенных текстов отдают огромные JSON.
  - **Правило:** при ошибке «exceeds maximum allowed tokens» — не пытаться читать сохранённый файл построчно. Использовать Python для парсинга JSON и извлечения нужных полей.

- **2026-05-18 —** Документация UI-kit заявляла 9 цветовых токенов, но `get_variable_defs` вернул `{}`.
  - **Почему:** дизайнеры часто описывают цветовые «токены» как визуальный список в Figma, не создавая настоящие Variables.
  - **Правило:** при ревизии DS первый шаг — `get_variable_defs`. Документация и реальность могут расходиться.

- **2026-05-18 (внешний источник, meta-pp template) —** Subagent'ы Claude Code (через `.claude/agents/*.md` как отдельные процессы) НЕ наследуют подключение к MCP-серверам, даже если перечислить `mcp__*` тулзы в `tools:`.
  - **Почему:** MCP-серверы аутентифицируются в процессе родительского агента. Subagent запускается изолированно. Поле `tools:` только фильтрует список тулзов, не создаёт сессию.
  - **Правило:** все Figma-операции исполняет родительский агент. У нас «агенты» = роли через переключение, не настоящие Claude Code subagents.
