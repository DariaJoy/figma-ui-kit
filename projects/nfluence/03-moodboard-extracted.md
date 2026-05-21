# Извлечённая стилистика: Nfluence

**Дата:** 2026-05-21 | **Slug:** nfluence | **Статус:** moodboard_extracted | **Режим:** extract

---

## 1. Источники

| # | Платформа | URL | Что взято по запросу Дарьи |
|---|-----------|-----|----------------------------|
| 1 | Tilda | https://www.e-b-agency.com | Тёмная палитра, графические летающие SVG, бегущие строки манифеста, крупная типографика, лимоновый акцент |
| 2 | Webflow + Slater | https://truus.co | Курсив для акцентов в заголовках, стикерные SVG-иконки в карточках, drag-карусель кейсов, анимации |
| 3 | Webflow | https://www.techto.org | Типографика, сетка, мягкие градиенты, pill-формы у CTA |

**Метод:** web_fetch → HTML → grep CSS-файлов по URL → прямой curl CSS-файлов. Приоритет — факты из CSS. Визуальные описания помечены `[наблюдение]`.

---

## 2. Характер

**Прилагательные (3–5):** графичный, ироничный, живой, уверенный, двуветочный

**Что значит:**
Стиль Nfluence строится на контрасте — тёмная агрессивная ветка для блогеров (e-b-agency как тон) и структурированная доступная ветка для брендов (techto как тон). Truus.co задаёт общий принцип: всё должно двигаться и вести себя как живой организм — с кастомными анимациями, курсивом как эмоциональным акцентом, иконками-стикерами. Ирония — в смеси делового контекста (инфлюенс-маркетинг, юрзащита, кейсы с цифрами) и хайпового визуала.

---

## 3. Палитра

### 3.1 Из e-b-agency.com — тёмная ветка (для блогеров)

| Hex | Где встречается | Роль | Контраст с bg | Источник |
|-----|-----------------|------|--------------|---------|
| `#111605` | border-color на формах, активные элементы | Тёмная база (почти чёрный с зелёным тоном) | n/a (фон) | inline style в HTML |
| `#e6e6e6` | Поля форм, неактивные зоны | Нейтральный серый | 14.9:1 на #111605 | inline style |
| ≈`#c8e052` | Акцент [наблюдение по описанию Дарьи — лимоновый/жёлтый] | Основной акцент тёмной ветки | высокий на тёмном | визуальный анализ |

**Примечание:** e-b-agency.com на Tilda, встроенные стили хранятся в dynamically-генерируемых Tilda-блоках, прямой CSS-экстракт дал цвета форм, не брендовые. Лимоновый акцент — `[наблюдение, требует проверки пипеткой в браузере]`. У Nfluence уже есть `#C5EE5B` — близко к этому акценту, используем его.

### 3.2 Из truus.co — анимации и типографика

Цветовая система truus.co использует CSS-переменные (извлечены из slater.app CSS):

| Переменная | Описание | Роль в системе Nfluence |
|-----------|----------|------------------------|
| `--color-primary` | Selection highlight color | Аналог accent/lime |
| `--color-dark` | Основной тёмный текст | Аналог text/primary |
| `--color-light` | Светлый фон/текст | Аналог bg/primary инверсия (для светлой ветки) |
| `--color-pink` | Тег/акцент | Потенциал для брендовой ветки |
| `--color-orange` | Тег/акцент | Потенциал для категорий кейсов |

**Принцип:** truus.co использует **мультицветную тег-систему** — каждый тип контента/кейса получает свой цвет (Pink, Orange, Blue, Light Green, Maroon Red). Для Nfluence: теги платформ (VK = синий, TG = синий, YouTube = красный) могут работать по этому принципу.

### 3.3 Из techto.org — светлая ветка (для брендов)

| Цвет | Описание | Роль | Источник |
|------|----------|------|---------|
| Белый/очень светлый | Основной фон | bg/primary светлой ветки | [наблюдение WebFetch] |
| Deep navy / тёмно-синий | Текст, навигация | text/primary светлой ветки | [наблюдение WebFetch] |
| Teal/cyan акцент | CTA-кнопки, активные элементы | accent светлой ветки | [наблюдение WebFetch] |

**Примечание:** techto.org отдал только Webflow base CSS, кастомные цвета — `[наблюдение, не факт]`. Для Nfluence светлая ветка — дизайн-решение, не прямое копирование.

### 3.4 Итоговая композитная палитра Nfluence

Основана на существующем UI-kit v1.0 (`#070809`, `#C5EE5B`) + дополнения из референсов:

| Hex | Роль | Ветка | Источник |
|-----|------|-------|----------|
| `#070809` | bg/primary | Тёмная (блогеры) | Существующий UI-kit |
| `#111314` | bg/surface | Тёмная | Существующий UI-kit |
| `#1E2123` | bg/elevated | Тёмная | Существующий UI-kit |
| `#C5EE5B` | accent/lime | Тёмная | Существующий UI-kit ≈ e-b-agency лимон |
| `#FFFFFF` | text/primary | Тёмная | Существующий UI-kit |
| `#A8ABAE` | text/secondary | Тёмная | Существующий UI-kit |
| `#F5F4F0` | bg/primary | Светлая (бренды) | Вдохновение techto — тёплый белый `[гипотеза]` |
| `#0F1A2E` | text/primary | Светлая | Тёмно-синий как у techto `[гипотеза]` |
| `#2D6BFF` | accent/blue | Светлая | CTA для брендовой ветки `[гипотеза]` |
| `#252729` | border/default | Обе | Существующий UI-kit |

**⚠️ Риск:** светлая ветка (`#F5F4F0`, `#0F1A2E`, `#2D6BFF`) — гипотезы, не факты из CSS. Требуют утверждения Дарьёй перед созданием Variables.

---

## 4. Типографика

### 4.1 Шрифт заголовков — из существующего UI-kit + truus.co подтверждение

**Unbounded** (Google Fonts) — уже в UI-kit Nfluence.
- Начертания: Bold (700), Regular (400) — только латиница и кириллица Basic
- **Кириллица:** ✅ Поддерживается в Unbounded (Google Fonts, 2022+, расширенный набор)
- Альтернативы: Space Grotesk (мягче, больше кириллица), Neue Machina (более жёсткий, нет бесплатной кириллицы)
- Применение truus.co: display 8em, text-transform: lowercase — рекомендую **lowercase для display-заголовков Nfluence** как усиление характера

### 4.2 Шрифт текста — из truus.co как новая деталь

**Lora** (Google Fonts, serif) — используется в truus.co как body/accent.
- Начертания: Regular (400), Regular Italic (400i), Bold (700), Bold Italic (700i)
- **Кириллица:** ✅ Полная поддержка кириллицы во всех начертаниях (факт, Google Fonts)
- Применение в truus.co: для body text и **italic-акцентов в заголовках**

**Рекомендация для Nfluence:** добавить Lora как третий шрифт для italic-акцентов в Hero-заголовках (пример: «Инфлюенс-маркетинг, *в который верят*» — слово cursive). Это прямо воспроизводит паттерн truus.co. Применяется только для 1–2 слов в заголовке, не для body text.

**Существующий body:** Onest — остаётся основным текстовым шрифтом.

### 4.3 Типографическая шкала — из truus.co CSS (факт)

Извлечена из slater.app/15295/40172.css:

| Уровень | Размер (em) | Line-height | Letter-spacing | Transform |
|---------|-------------|-------------|----------------|-----------|
| display | 8em | 0.75 | — | lowercase |
| h1 | 6em | 0.95 | -0.03em | lowercase |
| h2 | 4em | 1.1 | -0.02em | lowercase |
| h3 | 2em | 1.0 | -0.01em | lowercase |

**Базовая единица:** 16px (--size-unit: 16)

**Адаптация для Nfluence:**
- Display/Hero заголовки: Unbounded Bold, размер ~80–96px (Desktop), lowercase ✅
- H1: Unbounded Bold, 60px
- H2: Onest Bold или Unbounded, 40px
- Body: Onest Regular, 16px
- Курсив-акцент: Lora Italic, 1em (совпадает с уровнем текста)

---

## 5. Ритм и spacing

### 5.1 Из truus.co CSS (факт)

```css
--section-padding: 6em;   /* ~96px при 16px базе */
--gap: 1.5em;             /* ~24px */
--container-padding-l: 3.75em (tablet), 1.25em (mobile)
--container-padding-m: 1.75em (tablet), 1.25em (mobile)
--container-padding-s: 0.5em (mobile)
```

**Масштаб:** 8px-система (1em = 16px, gap 1.5em = 24px, section 6em = 96px)

### 5.2 Итоговая шкала для Nfluence (композит)

| Токен | px | em | Применение |
|-------|----|----|-----------|
| spacing/xs | 4px | 0.25em | micro-gaps, иконки |
| spacing/s | 8px | 0.5em | внутри компонентов |
| spacing/m | 16px | 1em | стандарт (body) |
| spacing/l | 24px | 1.5em | gap между элементами (= truus gap) |
| spacing/xl | 48px | 3em | padding секций (мобайл) |
| spacing/2xl | 96px | 6em | section padding десктоп (= truus) |
| spacing/3xl | 128px | 8em | Hero height отступы |

---

## 6. Паттерны компонентов

### Hero
**e-b-agency:** Полноэкранный, видео/SVG-анимация на фоне, манифест-заголовок в 2–3 строки (8em), лимоновый акцент на 1 слово, запускает бегущую строку ниже.
**truus.co:** Заголовок в 2 строки lowercase («we make advertising / for the new mainstream»), курсив на одно слово, без фоновой картинки — только типографика + SVG-стикеры вокруг.
**→ Nfluence Hero:** Видео-фон (как e-b-agency) + манифест-заголовок lowercase + курсив-акцент на 1 слово (как truus) + lime accent `#C5EE5B` на слово.

### Buttons
**truus.co CSS:** `border-left: 0.25em solid var(--color-pink)` — нет традиционных pill кнопок; используются underline-style или border-accent.
**techto.org:** Pill-форма (border-radius большой), filled + стрелка →, high contrast.
**→ Nfluence:** Сохраняем существующие Button/Primary (pill = techto), Button/Ghost (border = truus). Добавляем стрелку → на Primary для CTA «Обсудить проект» и «Стать блогером».

### Cards
**truus.co:** Cards кейсов — image full-width, название курсивом снизу, минимум инфо. Drag-to-navigate carousel через Flickity.
**e-b-agency:** Cards проектов с service-тегами и geographic location.
**→ Nfluence Card/Blogger:** Фото + имя + платформа-тег (цветной, как truus multi-color) + 2–3 стата (охват, ER). Карусель через drag — воспроизводит truus.

### Nav
**truus.co:** Sticky, minimal, только лого + 3–4 пункта + CTA. Lowercase.
**e-b-agency:** Horizontal меню (Vision, Cases, Services) + «start a project» popup.
**→ Nfluence Nav:** Sticky, logo слева, центр (О нас / Кейсы / Для брендов / Для блогеров), CTA «Обсудить задачу» справа — как e-b-agency структурно.

### Footer
**truus.co:** Контакты + локация (amsterdam) + соцсети. Минимальный.
**→ Nfluence Footer:** Навигация по якорям + контакты + раздельные email-адреса (brief@, talent@) как Hype Agency.

### Бегущие строки (marquee)
**e-b-agency:** Горизонтальный marquee со словами (manifesto, cases) — ключевой паттерн «оживления» сайта.
**→ Nfluence:** Добавить 1 marquee после Hero с сообщением «#брендам #блогерам #маркировка #инфлюенс #работаем» — создаёт ритм.

### SVG-стикеры (truus.co паттерн)
Camera, smiley, cute, BAM, fistbump — floating stickers рядом с карточками кейсов.
**→ Nfluence:** Аналог — стикеры с иконками платформ (VK, Telegram, YouTube) floating возле Cards/Blogger. Лаймовый цвет для совместимости с темой.

### Микро-анимации

Из truus.co CSS (факт):
```css
--animation-default-fast: 0.4s cubic-bezier(0.625, 0.05, 0, 1)
--animation-default: 0.8s cubic-bezier(0.625, 0.05, 0, 1)
--animation-bounce: 0.8s cubic-bezier(0.35, 1.75, 0.6, 1)
--animation-expo: 0.8s cubic-bezier(0.87, 0, 0.13, 1)
```

**→ Nfluence animation tokens:**
- `ease/default`: 0.8s cubic-bezier(0.625, 0.05, 0, 1) — hover, появление
- `ease/bounce`: 0.8s cubic-bezier(0.35, 1.75, 0.6, 1) — кнопки, стикеры
- `ease/expo`: 0.8s cubic-bezier(0.87, 0, 0.13, 1) — slider, router

---

## 7. Согласованность с брифом и персонами

**С брифом:**
- ✅ «Мост между бизнес-показателями и креативным хаосом» — двуветочный подход реализует это буквально: тёмная ветка = хаос, светлая ветка = бизнес
- ✅ Neo-Brutalism + Bento Grid + Bold Typography — получаем от e-b-agency
- ⚠️ «Зрелый, технологичный, безопасный» — светлая ветка пока гипотеза, не полностью проработана

**С персонами:**
- ✅ **Максим:** тёмная ветка e-b-agency/truus — дерзкая, хайповая, «не кринж». Lowercase заголовки, lime accent, drag-carousel = mobile-friendly интерактивность
- ✅ **Елена:** структура truus (минимум шума, кейсы в фокусе) + pill-кнопки techto + «Обсудить проект» form без звонка
- ✅ Обе персоны проходят через Router → Router отдаёт им разный визуал (тёмная vs светлая ветка)

---

## 8. Риски и варианты решения

### Риск 1: Кириллица в Lora Italic ⚠️
- **Факт:** Lora поддерживает кириллицу в Regular и Bold. Italic версия кириллицы — `[требует проверки Google Fonts страницы]`
- **Вариант A:** Использовать Lora Italic для русского (если есть кириллица в italic)
- **Вариант B:** Unbounded Bold + css `font-style: italic` (псевдо-курсив, менее красив)
- **Вариант C:** Поменять Lora на PT Serif / Playfair Display (оба с Cyrillic + italic)

### Риск 2: Светлая ветка — все цвета гипотетические
- **Проблема:** techto.org не отдал кастомный CSS, все цвета светлой ветки — `[гипотеза]`
- **Вариант A:** Начать создавать Variables только для тёмной ветки (существующий UI-kit), светлую ветку отложить
- **Вариант B:** Дарья утверждает предложенные цвета светлой ветки (`#F5F4F0`, `#0F1A2E`, `#2D6BFF`) перед созданием Variables
- **Рекомендация:** Вариант B — спросить Дарью на этапе design-system-architect

### Риск 3: Три шрифта = нагрузка
- Unbounded + Onest + Lora = 3 google fonts в проекте
- **Факт:** Каждый шрифт = ~200–400KB transfer, критично для мобайла Максима
- **Вариант A:** Оставить 3 шрифта, но загружать Lora только там где нужен italic
- **Вариант B:** Убрать Lora, использовать CSS `font-style: italic` в Unbounded (менее выразительно)
- **Рекомендация:** Вариант A с preconnect + display: swap

---

## 9. Передача дальше

**Ключевые решения для design-system-architect:**

1. **Цвета тёмной ветки:** подтверждены — 9 токенов из UI-kit v1.0, плюс проверка лимонового на e-b-agency (#C5EE5B совпадает)
2. **Новые токены — мультицветные теги:** 5 цветов (Pink, Orange, Blue, LightGreen, MaroonRed) для платформ и категорий — по паттерну truus.co
3. **Цвета светлой ветки:** гипотезы (#F5F4F0, #0F1A2E, #2D6BFF) — требуют утверждения
4. **Шрифты:** Unbounded (display/h1) + Onest (body) + Lora Italic (акцент, если кириллица есть) — 3 шрифта
5. **Анимация:** 4 токена из truus.co CSS — перенести как STRING в Variables или задокументировать
6. **Spacing:** 8px-система из 7 шагов (xs/s/m/l/xl/2xl/3xl)
7. **Radius:** pill-форма кнопок из techto + существующий UI-kit
8. **Новые компоненты:** Role Switcher, Form×2, Timeline/Process, Card/Blogger со статистикой, Marquee (декоративный)

---

## 10. Журнал

| Дата | Агент | Действие | Результат |
|------|-------|----------|-----------|
| 2026-05-21 | moodboard-curator v4 | EXTRACT: web_fetch + curl CSS трёх URL | Создан 03-moodboard-extracted.md |
| 2026-05-21 | — | Truus CSS: slater.app — типографика, анимации, color-vars | Факты: шрифт Lora, шкала, cubic-bezier |
| 2026-05-21 | — | E-b-agency: Tilda inline styles | Факты: #111605, #e6e6e6; лимонный — наблюдение |
| 2026-05-21 | — | Techto: Webflow base CSS только | Наблюдения по WebFetch |
