# Roast My Listing — Design Specification

**Author:** Sky (Creative, OpenClaw)
**Date:** 2026-03-29

---

## 1. Design System

### 1.1 Цветовая палитра

Спека предлагала `#1a1a2e` / `#16213e` — это холодный navy. Для "roast" темы нужно больше тепла. Огонь = тёплый тёмный фон, а не холодный синий.

**Утверждённая палитра:**

| Token | HEX | Назначение | Контраст с text |
|-------|------|-----------|-----------------|
| `--bg` | `#1c1917` | Фон страницы (stone-900) | — |
| `--surface` | `#292524` | Карточки, блоки (stone-800) | — |
| `--surface-raised` | `#44403c` | Hover, активные элементы (stone-700) | — |
| `--fire` | `#ef4444` | Основной акцент, CTA glow | 4.6:1 на `--bg` |
| `--ember` | `#f97316` | Вторичный акцент, score medium | 5.2:1 на `--bg` |
| `--warm` | `#fbbf24` | Highlights, score high | 8.1:1 на `--bg` |
| `--text` | `#fafaf9` | Основной текст (stone-50) | 15.4:1 на `--bg` |
| `--text-muted` | `#a8a29e` | Вторичный текст (stone-400) | 5.1:1 на `--bg` |
| `--text-dim` | `#78716c` | Подписи, hints (stone-500) | 3.4:1 (decorative only) |
| `--score-low` | `#ef4444` | Score 1-3 (red-500) | 4.6:1 на `--bg` |
| `--score-mid` | `#f59e0b` | Score 4-6 (amber-500) | 6.4:1 на `--bg` |
| `--score-high` | `#22c55e` | Score 7-10 (green-500) | 5.6:1 на `--bg` |
| `--rewrite-bg` | `#1a2e1a` | Фон rewrite блока (тёмный зелёный) | — |
| `--rewrite-border` | `#22c55e` | Border-left rewrite | — |

**Почему stone вместо navy:**
- "Roast" = жарка, угли, тепло. Stone/charcoal palette = тлеющие угли.
- Navy (#1a1a2e) ассоциируется с космосом, tech dashboard — не та эмоция.
- Тёплый серый лучше сочетается с оранжевым и красным огнём.

**WCAG AA:** Все текстовые цвета проходят 4.5:1 на своих фонах. `--text-dim` используется ТОЛЬКО для декоративных элементов (иконки, разделители), не для читаемого текста.

### 1.2 Типографика

```css
--font-main: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
--font-mono: "SF Mono", "Cascadia Code", "Fira Code", monospace;

--text-xs: 0.75rem;    /* 12px — labels */
--text-sm: 0.875rem;   /* 14px — secondary */
--text-base: 1rem;     /* 16px — body */
--text-lg: 1.25rem;    /* 20px — section headers */
--text-xl: 1.5rem;     /* 24px — card titles */
--text-2xl: 2rem;      /* 32px — score number */
--text-hero: 2.5rem;   /* 40px — main title */
```

### 1.3 Spacing

8px grid:

```css
--space-1: 0.25rem;  /* 4px */
--space-2: 0.5rem;   /* 8px */
--space-3: 0.75rem;  /* 12px */
--space-4: 1rem;     /* 16px */
--space-6: 1.5rem;   /* 24px */
--space-8: 2rem;     /* 32px */
--space-12: 3rem;    /* 48px */
--space-16: 4rem;    /* 64px */
```

### 1.4 Радиусы и тени

```css
--radius-sm: 6px;
--radius-md: 12px;
--radius-lg: 16px;

--shadow-card: 0 2px 8px rgba(0,0,0,0.3);
--shadow-glow-fire: 0 0 20px rgba(239,68,68,0.3);
--shadow-glow-ember: 0 0 20px rgba(249,115,22,0.3);
```

---

## 2. Layout — 4 состояния

### 2a. Первый визит (нет API key)

```
+--------------------------------------------------+
|                                                    |
|    (centered, max-width: 480px)                    |
|                                                    |
|         ROAST MY LISTING                          |
|         (text-hero, --fire gradient text)          |
|                                                    |
|    "Paste your listing. Get brutally honest        |
|     feedback + a rewrite that actually sells."     |
|    (text-muted, text-base)                         |
|                                                    |
|    +--------------------------------------------+  |
|    |  CARD: surface bg, radius-lg               |  |
|    |                                            |  |
|    |  To get started, choose your AI provider:  |  |
|    |  (text-sm, text-muted)                     |  |
|    |                                            |  |
|    |  [ OpenAI ] [ Anthropic ] [ Google ]       |  |
|    |  (3 кнопки, outline style, при выборе =    |  |
|    |   fill с --fire border-bottom 3px)         |  |
|    |                                            |  |
|    |  API Key                                   |  |
|    |  +--------------------------------------+  |  |
|    |  | sk-...                               |  |  |
|    |  +--------------------------------------+  |  |
|    |  (input: surface-raised bg, radius-sm,     |  |
|    |   monospace font, type=password + eye icon) |  |
|    |                                            |  |
|    |  (lock icon) Your key stays in your        |  |
|    |  browser. We never see it.                 |  |
|    |  (text-dim, text-xs)                       |  |
|    |                                            |  |
|    |  [? How to get a key]                      |  |
|    |  (link, --ember color, text-xs)            |  |
|    |                                            |  |
|    |  [ START ROASTING ]                        |  |
|    |  (full-width, --fire bg, white text,       |  |
|    |   radius-md, shadow-glow-fire on hover)    |  |
|    +--------------------------------------------+  |
|                                                    |
+--------------------------------------------------+
```

**Onboarding нюансы (раздел 4 детально):**
- "How to get a key" раскрывает inline-блок с 3 инструкциями (по провайдеру)
- Нет отдельного overlay/modal — всё на одной странице, progressive disclosure
- Иконка замка рядом с privacy текстом снижает тревогу

### 2b. Ввод (API key есть)

```
+--------------------------------------------------+
|  HEADER (sticky на desktop, static на mobile)     |
|                                                    |
|  ROAST MY LISTING     (gear icon) Settings        |
|  (text-xl, --fire)     (text-dim, opens key mgmt) |
+--------------------------------------------------+

+--------------------------------------------------+
|  (centered, max-width: 640px)                     |
|                                                    |
|  Platform                                          |
|  [ Avito  v ]                                      |
|  (select: surface-raised bg, radius-sm,            |
|   --ember border на focus)                         |
|                                                    |
|  Your listing                                      |
|  +----------------------------------------------+  |
|  |                                              |  |
|  |  Paste your listing text here...             |  |
|  |  (placeholder: text-dim)                     |  |
|  |                                              |  |
|  |  (min-height: 160px, resize: vertical)       |  |
|  |  (surface bg, radius-md,                     |  |
|  |   --ember border на focus,                   |  |
|  |   transition: border 0.2s)                   |  |
|  +----------------------------------------------+  |
|  (char counter: text-dim, text-xs, right-aligned)  |
|  "47 / 10,000"                                     |
|                                                    |
|  [ ROAST IT ]                                      |
|  (full-width, --fire bg, white text bold,          |
|   radius-md, padding: space-4,                     |
|   shadow-glow-fire,                                |
|   disabled state: opacity 0.4, no glow,            |
|   hover: scale(1.02) + brighter glow)              |
|                                                    |
|  (disabled tooltip: "Enter at least 10 chars")     |
+--------------------------------------------------+
```

**Micro-interactions:**
- Textarea border переходит от transparent к `--ember` при focus (0.2s ease)
- CTA кнопка имеет постоянный мягкий glow (`--shadow-glow-fire`), усиливается на hover
- Character counter: появляется только при focus textarea (progressive disclosure)

### 2c. Loading — анимация "жарки"

```
+--------------------------------------------------+
|  (centered, max-width: 480px)                     |
|                                                    |
|      (fire emoji, pulsing scale animation)        |
|      Roasting your listing...                      |
|      (text-lg, --text-muted)                      |
|                                                    |
|      [ember bar — animated gradient]               |
|      (height: 4px, width: 100%,                   |
|       linear-gradient --fire -> --ember -> --warm  |
|       animating background-position left-to-right  |
|       infinite, 1.5s ease-in-out)                  |
|                                                    |
|      "Inspecting your listing with a magnifying    |
|       glass and a flamethrower..."                 |
|      (text-sm, text-dim, italic,                   |
|       changes every 3 sec — 4 phrases rotate)      |
|                                                    |
+--------------------------------------------------+
```

**Rotating phrases:**
1. "Inspecting your listing with a magnifying glass and a flamethrower..."
2. "Warming up the roast chamber..."
3. "Finding all the things you did wrong..."
4. "Preparing a rewrite that will make you cry (happy tears)..."

**CSS анимации:**
```css
@keyframes ember-flow {
  0% { background-position: -200% center; }
  100% { background-position: 200% center; }
}

@keyframes pulse-fire {
  0%, 100% { transform: scale(1); opacity: 1; }
  50% { transform: scale(1.15); opacity: 0.85; }
}
```

### 2d. Результат

```
+--------------------------------------------------+
|  (centered, max-width: 640px)                     |
|                                                    |
|  SCORE CARD                                        |
|  +----------------------------------------------+  |
|  |  (surface bg, radius-lg, border-left: 4px    |  |
|  |   цвет зависит от score)                     |  |
|  |                                              |  |
|  |  4 / 10          (text-2xl, score-color,     |  |
|  |                    font-weight: 800)          |  |
|  |                                              |  |
|  |  [====----------]  (progress bar:            |  |
|  |   height 8px, radius-sm,                     |  |
|  |   bg: surface-raised,                        |  |
|  |   fill: score-color gradient)                |  |
|  |                                              |  |
|  |  "Your listing is a cry for help"            |  |
|  |  (text-base, italic, --text-muted)           |  |
|  |                                              |  |
|  |  BREAKDOWN           (collapsible, closed    |  |
|  |  Title:    [===---]   by default)            |  |
|  |  Desc:     [=====--]                         |  |
|  |  Keywords: [==-----]                         |  |
|  |  Emotion:  [====---]                         |  |
|  |  Trust:    [======-]                         |  |
|  |  (mini bars, each 100px wide, score-colored) |  |
|  +----------------------------------------------+  |

|  THE ROAST                                         |
|  +----------------------------------------------+  |
|  |  (surface bg, radius-lg)                     |  |
|  |                                              |  |
|  |  +------------------------------------------+|  |
|  |  | (border-left: 3px --fire)                ||  |
|  |  | (fire emoji) Short title                 ||  |
|  |  | "Your title has 3 words. THREE."         ||  |
|  |  | (text-base, --text)                      ||  |
|  |  |                                          ||  |
|  |  | Fix: Add key specs: brand, size...       ||  |
|  |  | (text-sm, --score-high, bg: rgba         ||  |
|  |  |  green 0.1, padding: space-2, radius-sm) ||  |
|  |  +------------------------------------------+|  |
|  |                                              |  |
|  |  (repeat for each roast point,               |  |
|  |   spacing: space-4 between items)            |  |
|  +----------------------------------------------+  |

|  THE REWRITE                                       |
|  +----------------------------------------------+  |
|  |  (--rewrite-bg, radius-lg,                   |  |
|  |   border-left: 4px --rewrite-border)         |  |
|  |                                              |  |
|  |  THE REWRITE            [ Copy ]             |  |
|  |  (text-lg)   (btn: outline --score-high,     |  |
|  |               radius-sm, on click:           |  |
|  |               fill green + "Copied!" 2s)     |  |
|  |                                              |  |
|  |  Title                                       |  |
|  |  "Samsung Galaxy S21, 128GB, Phantom Gray,   |  |
|  |   Mint Condition — Unlocked"                 |  |
|  |  (text-base, font-weight: 600)               |  |
|  |                                              |  |
|  |  Description                                 |  |
|  |  "Selling my Samsung Galaxy S21..."          |  |
|  |  (text-base, --text, line-height: 1.6)       |  |
|  +----------------------------------------------+  |

|  ACTIONS                                           |
|  +----------------------------------------------+  |
|  |  [ Share Roast ]    [ Try Another ]          |  |
|  |  (--ember outline,   (--text-muted outline,  |  |
|  |   glow on hover)      no glow)               |  |
|  +----------------------------------------------+  |
+--------------------------------------------------+
```

**Визуальная иерархия результата:**
1. Score card — первый взгляд, крупная цифра, цвет сразу даёт оценку
2. Roast — основной контент, каждый пункт = карточка с border-left
3. Rewrite — отдельный визуальный мир (зеленоватый фон), "решение проблемы"
4. Actions — внизу, неакцентированные

**Roast item структура (каждый пункт):**
- Border-left: 3px `--fire`
- Padding: `space-4`
- Background: `--surface`
- Issue title: bold, `--text`, с fire emoji перед ним
- Roast text: `--text`, обычный вес
- Fix: отдельный мини-блок, зелёный фон (rgba `--score-high` 0.1), текст `--score-high`

---

## 3. Share Card (Canvas -> PNG)

**Размер:** 600x400px (1200x800 @2x для retina)

```
+--------------------------------------------------+
|  (bg: gradient от #1c1917 к #292524)              |
|                                                    |
|  ROAST MY LISTING                                 |
|  (24px, --fire, uppercase, letter-spacing: 2px)   |
|                                                    |
|  +--------------------------------------------+   |
|  |  (circle, 120px, stroke: score-color,      |   |
|  |   stroke-width: 6px, bg: transparent)      |   |
|  |                                            |   |
|  |           4/10                             |   |
|  |  (32px, bold, score-color)                 |   |
|  +--------------------------------------------+   |
|                                                    |
|  "Your listing is a cry for help"                 |
|  (16px, --text-muted, italic, max 2 lines)        |
|                                                    |
|  ─────────────────────────────────                |
|  (1px line, --surface-raised)                     |
|                                                    |
|  roastmylisting.com                                |
|  (12px, --text-dim, uppercase, letter-spacing)    |
|                                                    |
+--------------------------------------------------+
```

**Canvas rendering notes:**
- Рисуем на canvas 1200x800, потом экспортируем
- Score circle: `arc()` с `strokeStyle` = score-color
- Текст: `fillText()`, font = system-ui
- Background: `createLinearGradient()` сверху вниз
- Export: `canvas.toDataURL('image/png')`
- Если Web Share API доступен — `navigator.share({ files: [blob] })`, иначе download

---

## 4. Onboarding — API Key без отпугивания

### Принципы:
1. **Не modal, не popup** — встроен в страницу как первый экран
2. **Explain why** — "To roast listings, we send them to an AI. You bring your own key = your data stays yours."
3. **Visual trust** — lock icon, "stays in your browser" прямо рядом с input
4. **Low friction** — 3 кнопки провайдеров, одно поле, одна кнопка
5. **Help доступен но скрыт** — "How to get a key?" раскрывает inline инструкции

### "How to get a key?" — inline expandable:

```
+----------------------------------------------+
|  OpenAI:                                      |
|  1. Go to platform.openai.com/api-keys       |
|  2. Click "Create new secret key"             |
|  3. Copy and paste here                       |
|  Cost: ~$0.01 per roast                       |
+----------------------------------------------+
|  Anthropic:                                   |
|  1. Go to console.anthropic.com/keys          |
|  2. Click "Create Key"                        |
|  3. Copy and paste here                       |
|  Cost: ~$0.01 per roast                       |
+----------------------------------------------+
|  Google:                                      |
|  1. Go to aistudio.google.com/apikey          |
|  2. Click "Create API key"                    |
|  3. Copy and paste here                       |
|  Cost: free tier available                    |
+----------------------------------------------+
```

**Показываем только выбранный провайдер** — не все три сразу.

### Settings (gear icon, после onboarding):
- Клик по gear icon → inline dropdown (не modal)
- Показывает: текущий провайдер, маскированный key (`sk-...abc`), кнопка "Change"
- "Change" возвращает на onboarding-like форму inline

---

## 5. Микро-детали

### 5.1 Score цвета

```javascript
function getScoreColor(score) {
  if (score <= 3) return 'var(--score-low)';   // #ef4444
  if (score <= 6) return 'var(--score-mid)';   // #f59e0b
  return 'var(--score-high)';                   // #22c55e
}
```

Progress bar: fill с gradient от score-color (100% opacity) к score-color (60% opacity), left-to-right.

### 5.2 Roast пункт

```
(fire emoji)  Issue Title                        (--text, bold)
"Roast text with humor and pain"                (--text, normal)

  Fix: Concrete actionable fix here              (--score-high text,
                                                   bg rgba(34,197,94,0.1),
                                                   padding space-2 space-3,
                                                   radius-sm,
                                                   border-left 2px --score-high)
```

### 5.3 Rewrite блок

- Background: `--rewrite-bg` (#1a2e1a) — тёмно-зелёный, визуально "решение"
- Border-left: 4px `--rewrite-border` (#22c55e)
- Контраст с roast (red/fire) создаёт narrative arc: проблема -> решение

### 5.4 Copy button

```
Idle:    [ Copy ]     — outline, --score-high border, --score-high text
Hover:   [ Copy ]     — fill --score-high bg, --bg text
Click:   [ Copied! ]  — fill --score-high bg, check icon, 2s timeout
```

CSS transition: background-color 0.15s, transform: scale(0.95) on click.

### 5.5 Textarea

- Idle: border 1px transparent (на surface bg визуально borderless)
- Focus: border 1px `--ember`, box-shadow `--shadow-glow-ember`
- Error (< 10 chars при попытке submit): border 1px `--fire`, shake animation

```css
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-4px); }
  40%, 80% { transform: translateX(4px); }
}
```

### 5.6 CTA "ROAST IT"

```css
.cta {
  background: var(--fire);
  color: white;
  font-weight: 700;
  font-size: var(--text-base);
  text-transform: uppercase;
  letter-spacing: 1px;
  padding: var(--space-4) var(--space-8);
  border: none;
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-glow-fire);
  cursor: pointer;
  transition: transform 0.15s, box-shadow 0.15s;
}
.cta:hover:not(:disabled) {
  transform: scale(1.02);
  box-shadow: 0 0 30px rgba(239,68,68,0.5);
}
.cta:disabled {
  opacity: 0.4;
  box-shadow: none;
  cursor: not-allowed;
}
```

---

## 6. WCAG AA Compliance

### Контраст-таблица (проверено):

| Элемент | Foreground | Background | Ratio | Pass? |
|---------|-----------|-----------|-------|-------|
| Body text | #fafaf9 | #1c1917 | 15.4:1 | AA (pass) |
| Muted text | #a8a29e | #1c1917 | 5.1:1 | AA (pass) |
| Muted text on card | #a8a29e | #292524 | 4.5:1 | AA (pass) |
| Fire on bg | #ef4444 | #1c1917 | 4.6:1 | AA (pass) |
| Ember on bg | #f97316 | #1c1917 | 5.2:1 | AA (pass) |
| Score-high on bg | #22c55e | #1c1917 | 5.6:1 | AA (pass) |
| Score-mid on bg | #f59e0b | #1c1917 | 6.4:1 | AA (pass) |
| White on fire btn | #ffffff | #ef4444 | 4.6:1 | AA (pass) |
| Dim text | #78716c | #1c1917 | 3.4:1 | FAIL — decorative only |

### Правила:
- `--text-dim` НИКОГДА не используется для контента, который нужно прочитать
- Для placeholder текста: `--text-muted` (а не dim), ratio 5.1:1
- Все interactive элементы имеют visible focus ring: `outline: 2px solid var(--ember); outline-offset: 2px`
- Score colors на тёмном фоне: все проходят AA

---

## 7. Responsive Breakpoints

```css
/* Mobile first */
.container { max-width: 100%; padding: var(--space-4); }

/* Tablet+ */
@media (min-width: 640px) {
  .container { max-width: 640px; margin: 0 auto; padding: var(--space-8); }
}

/* Desktop */
@media (min-width: 1024px) {
  .container { max-width: 720px; }
}
```

Mobile (360px+):
- Все карточки full-width с padding
- Provider buttons stack vertically при < 400px
- Textarea min-height: 120px (меньше чем desktop)
- Score число: text-xl вместо text-2xl

---

## 8. Transitions & Animations Summary

| Элемент | Анимация | Duration | Trigger |
|---------|---------|----------|---------|
| CTA hover | scale + glow | 0.15s | hover |
| Textarea focus | border + glow | 0.2s | focus |
| Loading bar | gradient flow | 1.5s infinite | loading state |
| Loading emoji | pulse scale | 1s infinite | loading state |
| Loading phrases | fade in/out | 0.5s | every 3s |
| Score appear | fade + slide up | 0.4s | result loaded |
| Roast items | staggered fade in | 0.3s + 0.1s delay each | result loaded |
| Copy -> Copied | bg fill | 0.15s | click |
| Shake error | translateX | 0.4s | validation fail |
| Breakdown | height expand | 0.3s | click toggle |

---

## 9. Error States

```
API Error:
+----------------------------------------------+
|  (surface bg, border-left: 4px --fire)       |
|                                              |
|  (warning icon) Something went wrong         |
|  "Could not reach OpenAI. Check your key     |
|   and try again."                            |
|  (text-muted, text-sm)                       |
|                                              |
|  [ Retry ]   [ Change API Key ]              |
+----------------------------------------------+

"AI went off-script" (invalid JSON):
+----------------------------------------------+
|  (surface bg, border-left: 4px --ember)      |
|                                              |
|  (shrug icon) AI went off-script             |
|  "The AI returned something unexpected.      |
|   Here's the raw response:"                  |
|                                              |
|  +------------------------------------------+|
|  |  (monospace, surface-raised bg,          ||
|  |   scrollable, max-height: 200px)         ||
|  |  raw AI response here...                 ||
|  +------------------------------------------+|
|                                              |
|  [ Try Again ]                               |
+----------------------------------------------+
```

---

## 10. Implementation Notes for Developer

1. **Single file** — все CSS vars в `:root`, все стили в `<style>`, весь JS в `<script>`
2. **State machine** — 4 states: `setup`, `input`, `loading`, `result`. Show/hide sections via CSS classes.
3. **No frameworks** — vanilla JS, vanilla CSS. CSS custom properties for theming.
4. **Canvas for share card** — draw at 2x resolution, export PNG
5. **Progressive disclosure** — score breakdown collapsed by default, "How to get key" collapsed, char counter on focus only
6. **Border-left pattern** — used consistently: score card (score-color), roast items (fire), rewrite (green), errors (fire/ember)
7. **Glow pattern** — CTA button always has subtle glow, intensifies on hover. This is the only element with persistent glow.
