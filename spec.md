# Roast My Listing — Product Specification

## Одна строка
Вставь объявление — получи жёсткий разбор + готовый улучшенный текст.

---

## WifiCard

- **Персона:** Настя, 28 лет. Продаёт б/у вещи на Avito/Etsy. Написала объявление, 0 откликов.
- **Функция:** Анализ текста объявления в формате "roast" + переписанная версия.
- **Враг:** Подруга которая говорит "нормально" / самостоятельный скроллинг чужих объявлений.
- **Эмоция:** Фрустрация ("почему не покупают?!") -> облегчение ("а, вот что не так!").
- **Результат:** Готовый текст объявления, который можно скопировать и вставить.

---

## User Flow

```
1. Открыл страницу
2. Видит: большое текстовое поле + выпадающий список площадки (Avito/Etsy/eBay/Airbnb/Other)
3. Вставляет текст объявления (или ссылку — парсим текст на клиенте если возможно)
4. Нажимает "ROAST IT"
5. Ждёт 3-5 сек (анимация огня/жарки)
6. Получает результат в 3 блоках:
   a. ROAST — 5-7 пунктов что плохо, с юмором но конструктивно
   b. SCORE — оценка 1-10 с прогресс-баром
   c. REWRITE — улучшенная версия объявления, кнопка "Copy"
7. Кнопка "Share Roast" — генерирует картинку-карточку с score для соцсетей
```

---

## Технический стек

### Архитектура: Single-file HTML + BYOK

```
[Browser] ---> [AI API напрямую]
   |
   index.html (весь код)
```

### Детали

- **Файл:** один `index.html`, self-contained
- **AI Provider:** пользователь вставляет свой API key (поддержка: OpenAI, Anthropic, Google Gemini)
- **API key хранение:** localStorage (с предупреждением что key остаётся в браузере)
- **Стили:** встроенный CSS, тёмная тема, "fire" акценты (оранжевый/красный)
- **JS:** vanilla, никаких фреймворков
- **Шрифт:** system fonts (без загрузки)

### API интеграция

Три провайдера, один промпт:

```javascript
// Поддерживаемые провайдеры
const PROVIDERS = {
  openai: { url: 'https://api.openai.com/v1/chat/completions', model: 'gpt-4o-mini' },
  anthropic: { url: 'https://api.anthropic.com/v1/messages', model: 'claude-sonnet-4-20250514' },
  google: { url: 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent', model: 'gemini-2.0-flash' }
};
```

### System Prompt (ядро продукта)

```
You are "The Listing Roaster" — a brutally honest but constructive marketplace listing critic.

Platform: {platform}

Analyze this listing and respond in EXACTLY this JSON format:
{
  "roast": [
    {"issue": "short title", "roast": "Your title has 3 words. THREE. Even a tweet has more effort.", "fix": "Add key specs: brand, size, condition, color"}
  ],
  "score": 4,
  "score_breakdown": {
    "title": 3,
    "description": 5,
    "keywords": 2,
    "emotion": 4,
    "trust": 6
  },
  "rewrite": {
    "title": "...",
    "description": "..."
  },
  "summary": "One line savage summary of the listing"
}

Rules:
- Be funny but USEFUL. Every roast must have a concrete fix.
- Score honestly: most listings deserve 3-6. A 9-10 is exceptional.
- Rewrite must be ready to copy-paste. Keep the same product, improve everything.
- Adapt to platform norms (Avito = Russian, Etsy = creative/SEO, eBay = specs-heavy, Airbnb = emotional/trust).
- If the listing is in Russian, respond in Russian.
- If the listing is in English, respond in English.
- Max 7 roast points.
```

---

## UI/UX Specification

### Layout

```
+------------------------------------------+
|        ROAST MY LISTING                  |
|        (fire emoji, tagline)              |
+------------------------------------------+
|                                          |
|  [Platform dropdown: Avito/Etsy/...]     |
|                                          |
|  +------------------------------------+  |
|  |                                    |  |
|  |   Paste your listing here...       |  |
|  |   (textarea, min 5 rows)           |  |
|  |                                    |  |
|  +------------------------------------+  |
|                                          |
|  [ ROAST IT ]  (big orange button)       |
|                                          |
+------------------------------------------+

--- РЕЗУЛЬТАТ (появляется после ответа) ---

+------------------------------------------+
|  SCORE: 4/10  [====------]               |
|  "Your listing is a cry for help"        |
+------------------------------------------+
|  THE ROAST                               |
|  1. Title: ... (fix: ...)                |
|  2. Photos: ... (fix: ...)               |
|  ...                                     |
+------------------------------------------+
|  THE REWRITE                    [COPY]   |
|  Title: ...                              |
|  Description: ...                        |
+------------------------------------------+
|  [Share Roast]  [Try Another]            |
+------------------------------------------+
```

### Первый запуск (нет API key)

```
+------------------------------------------+
|  Welcome! To roast listings, you need    |
|  an AI API key.                          |
|                                          |
|  [OpenAI]  [Anthropic]  [Google]         |
|                                          |
|  API Key: [__________________________]   |
|                                          |
|  Your key stays in your browser.         |
|  We never see it. [How to get a key?]    |
|                                          |
|  [Save & Start Roasting]                 |
+------------------------------------------+
```

### Share Card (генерация canvas -> PNG)

```
+---------------------------+
|  ROAST MY LISTING         |
|                           |
|  Score: 4/10              |
|  "Your listing needs CPR" |
|                           |
|  roastmylisting.com       |
+---------------------------+
```

---

## Цветовая палитра

- Background: `#1a1a2e`
- Card background: `#16213e`
- Primary (fire): `#e94560`
- Secondary (warm): `#f5a623`
- Text: `#eee`
- Muted text: `#888`
- Score colors: 1-3 red, 4-6 yellow, 7-10 green

---

## Acceptance Criteria

### AC-1: Ввод объявления
- [ ] Textarea принимает текст от 10 до 10,000 символов
- [ ] Dropdown позволяет выбрать площадку: Avito, Etsy, eBay, Airbnb, Other
- [ ] Кнопка "Roast It" неактивна если текст < 10 символов
- [ ] При нажатии показывается loading анимация

### AC-2: API Key Management
- [ ] Поддержка 3 провайдеров: OpenAI, Anthropic, Google Gemini
- [ ] Key сохраняется в localStorage
- [ ] При невалидном key показывается понятная ошибка
- [ ] Кнопка "Change key" в настройках (gear icon)
- [ ] Ссылка "How to get a key?" с инструкциями для каждого провайдера

### AC-3: Roast результат
- [ ] Score отображается как число X/10 + прогресс-бар с цветом
- [ ] Summary (одна строка) отображается под score
- [ ] 5-7 roast пунктов, каждый содержит: issue + roast + fix
- [ ] Score breakdown по категориям: title, description, keywords, emotion, trust

### AC-4: Rewrite
- [ ] Переписанный title + description отображаются в отдельном блоке
- [ ] Кнопка "Copy" копирует весь rewrite в буфер обмена
- [ ] После копирования кнопка меняется на "Copied!" на 2 секунды

### AC-5: Share
- [ ] Кнопка "Share Roast" генерирует PNG карточку через Canvas API
- [ ] Карточка содержит: score, summary, URL продукта
- [ ] Карточка скачивается или открывается Web Share API (если поддерживается)

### AC-6: Responsive
- [ ] Работает на мобильном (360px+)
- [ ] Работает на десктопе (1024px+)
- [ ] Textarea адаптируется по ширине

### AC-7: Error Handling
- [ ] Таймаут API (30 сек) -> показать ошибку + retry кнопка
- [ ] Невалидный JSON от AI -> показать raw текст с пометкой "AI went off-script"
- [ ] Rate limit -> показать сообщение "Too many roasts, wait a minute"
- [ ] Пустой текст -> валидация до отправки

### AC-8: Локализация
- [ ] Если текст объявления на русском, AI отвечает на русском
- [ ] Если на английском — на английском
- [ ] UI-интерфейс на английском (универсальный)

### AC-9: Performance
- [ ] Страница загружается < 1 сек (single file, no external deps)
- [ ] Первый roast работает < 10 сек (время ответа API)
- [ ] Общий размер HTML файла < 50KB

### AC-10: Privacy
- [ ] Никакие данные не отправляются никуда кроме выбранного AI API
- [ ] API key не покидает localStorage
- [ ] Нет analytics, трекеров, cookies (кроме localStorage)

---

## Что НЕ входит в v1

- Парсинг URL объявления (вставлять только текст)
- Истории roast-ов
- Сравнение "до/после" score
- Мультиязычный UI
- Бэкенд / прокси
- Встроенный бесплатный режим без API key

---

## Метрики успеха

- Пользователь получает roast за < 10 сек
- Roast содержит actionable пункты (не generic советы)
- Rewrite существенно отличается от оригинала
- Share карточка генерируется корректно

---

## Следующие шаги

1. Саня (продакт) -> передаёт спеку на дизайн
2. Дизайн -> code в single-file HTML
3. Audit -> fix -> re-audit
4. Деплой в sandbox
