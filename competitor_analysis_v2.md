# Roast My Listing -- Конкурентный анализ v2

**Автор:** Молот (разведка, OpenClaw)
**Дата:** 2026-03-29
**Формат:** RECON + COMPARISON
**Цель:** Найти фичи, которые поднимут оценку с 9.2 до 9.5 и дадут преимущество над платными конкурентами.

---

## 1. Карта конкурентов

Прямых конкурентов (AI roast/критика объявлений на маркетплейсах) -- **нет**. Это наша уникальная ниша. Конкуренты -- это инструменты оптимизации листингов, разбитые по платформам.

### Матрица конкурентов

| Конкурент | Платформа | Цена | Что есть | Чего нет | Наш план |
|---|---|---|---|---|---|
| **Marmalead** | Etsy | $19/мес ($190/год) | SEO-ключевики, тренды, ценовой анализ, AI-коуч "Marma", 3-мес прогнозы, ChatGPT shopping оптимизация | Критика текста, roast-формат, rewrite объявления, мульти-платформа | Мы даём то, чего нет: жёсткий разбор + готовый текст. Добавить Etsy-специфичные SEO-подсказки |
| **eRank** | Etsy | Free / $5.99-$29.99/мес | Listing audit, ключевики (200/день на Pro), трекинг конкурентов (50), тренды, Google Analytics интеграция | Нет AI-rewrite, нет roast, нет мульти-платформы | Их listing audit -- ближайший аналог, но без юмора и rewrite. Мы выигрываем форматом |
| **EverBee** | Etsy | Free / $29.99-$99/мес | Product research, keyword analysis (volume + competition score), shop analyzer (4M+ stores), Google Trends | Нет критики текста, нет rewrite, нет мульти-платформы | Их данные дополняют наш анализ. Мы можем рекомендовать: "use EverBee for research, us for copy" |
| **Helium 10** | Amazon | $129-$359/мес | Listing Analyzer (Listing Quality Score), keyword research, competitor analysis, 50-150 аудитов/мес | Нет roast-формата, нет мульти-платформы, дорого | $129/мес vs $0. Мы бьём ценой. Добавить Amazon-специфичные метрики (backend keywords, bullet points) |
| **Perci AI** | Amazon | Подписка (mid-range, ~$30-50/мес) | AI listing generation, A9/A10 оптимизация, restricted keyword guard, bulk (10K+ SKU), A/B тесты (+56% продаж) | Нет критики/roast, нет мульти-платформы | Их restricted keyword guard -- хорошая идея для Amazon-промпта |
| **PriceLabs** | Airbnb/STR | $19.99/листинг/мес | Dynamic pricing, Listing Optimizer (AI-анализ фото/текста/аменити), 160+ PMS интеграций | Нет roast-формата, нет rewrite текста, только pricing-фокус | Airbnb в 2026 перешёл на NLP-поиск -- мы можем анализировать описания под новый алгоритм |
| **AutoRank** | Airbnb | $9/листинг/мес | Авто-оптимизация еженедельно, API в PMS, keyword trends, guest search behavior | Нет roast, нет instant-фидбэка, нужен PMS | Мы -- instant feedback без интеграций. Для хостов кто хочет быстрый чек |
| **Jurny (rbnb.ai)** | Airbnb/STR | Не раскрыта | AI Listing Optimizer, Airbnb-алгоритм, локальный бенчмарк, пошаговые улучшения | Нет roast-формата, нет бесплатного доступа | Формат "пошаговых улучшений" стоит изучить для UX |
| **Nifty AI** | eBay/Poshmark/Mercari/Depop/Etsy | $25-$89.99/мес | AI из фото (SEO-тайтлы, описания, хэштеги), crosslisting, автоматизация | Нет критики существующих листингов, только генерация новых | Мы анализируем существующий текст -- другая задача |
| **3Dsellers** | eBay | Подписка | AI-описания, кроссплатформа, автоматизация CS | Нет аудита/критики | Аналогично Nifty -- генерация, не анализ |
| **Roast My Web** | Сайты (не маркетплейсы) | От $4/аудит | PDF-отчёты, SEO/UX/CRO/mobile, white-label, брендинг | Для сайтов, не для листингов маркетплейсов | Духовный предок. Их PDF-отчёт -- идея для share-фичи |

---

## 2. Ключевые инсайты

### 2.1. У нас нет прямых конкурентов

Никто не делает **AI roast** (критика + юмор + rewrite) для маркетплейс-листингов. Существующие инструменты делятся на:
- **SEO/Research** (Marmalead, eRank, EverBee) -- данные и ключевики, но не текст
- **Listing Generation** (Nifty, 3Dsellers, Perci) -- создают новые листинги, не критикуют существующие
- **Pricing/Optimization** (PriceLabs, AutoRank) -- цены и алгоритмы, не копирайтинг
- **Audit** (eRank Listing Audit, Helium 10 Listing Analyzer) -- ближайшие аналоги, но без юмора и rewrite

### 2.2. Ценовое преимущество -- убийственное

| Категория | Средняя цена конкурентов | Roast My Listing |
|---|---|---|
| Etsy SEO | $6-30/мес | **$0** (BYOK) |
| Amazon optimization | $30-359/мес | **$0** (BYOK) |
| Airbnb optimization | $10-20/листинг/мес | **$0** (BYOK) |
| Стоимость 1 roast (OpenAI gpt-4o-mini) | -- | **~$0.001-0.005** |

Реальная стоимость для пользователя: доли цента за roast. Против $19-359/мес подписок.

### 2.3. Что конкуренты делают лучше нас (пока)

| Фича | Кто делает | Что именно |
|---|---|---|
| SEO keywords с данными | Marmalead, eRank, EverBee | Реальные search volumes, competition scores, тренды |
| Platform-specific алгоритмы | Perci (A9/A10), AutoRank (Airbnb NLP) | Оптимизация под конкретный алгоритм поиска |
| Фото-анализ | PriceLabs, Nifty (из фото -> листинг) | Оценка качества фото, рекомендации |
| A/B тестирование | Perci (+56% продаж в тестах) | Сравнение вариантов текста |
| Competitor tracking | eRank (50 конкурентов), EverBee (4M shops) | Мониторинг конкурентов |
| PDF-отчёты | Roast My Web | Брендированные отчёты для клиентов |

---

## 3. Рекомендация: ТОП-5 фич для v2

Ранжировано по соотношению "ценность / сложность реализации".

### TOP-1: Category-Specific Prompts (Effort: Low, Impact: High)

**Что:** Вместо generic "analyze this listing" -- промпты заточены под каждую платформу:
- **Etsy**: акцент на SEO-теги, storytelling, handmade-vibes, длина title (140 chars)
- **Amazon**: bullet points, backend keywords, A+ content hints, restricted phrases
- **eBay**: item specifics, condition description, shipping clarity
- **Airbnb**: NLP-ключевики (в 2026 Airbnb ищет по natural language), аменити, trust signals

**Почему:** Marmalead берёт $19/мес за Etsy-специфичные советы. Мы можем дать это бесплатно через умный промпт. Perci берёт деньги за Amazon A9/A10 знания -- мы закодируем это в system prompt.

**Как:** Расширить system prompt для каждой платформы. Добавить подкатегории (Etsy: jewelry vs digital downloads vs vintage).

### TOP-2: Before/After Compare (Effort: Low, Impact: High)

**Что:** Side-by-side view оригинального текста и rewrite. Подсвеченные различия (diff-like). Процент улучшения по каждой метрике.

**Почему:** Ни один конкурент не даёт instant visual diff. Это "wow-эффект" для share в соцсетях. Формат: "Score 3/10 -> 8/10" вирален по природе.

**Как:** Чистый фронтенд. Два столбца, цветовая подсветка изменений. Никакого бэкенда.

### TOP-3: SEO Keyword Suggestions (Effort: Medium, Impact: High)

**Что:** В результат roast добавить блок "Suggested Keywords" с 5-10 ключевиками для платформы. AI генерирует их на основе текста + знания платформы.

**Почему:** Это основная фича Marmalead ($19/мес) и eRank ($10-30/мес). Мы не дадим search volume (у нас нет данных), но AI может предложить релевантные ключевики на основе категории и текста. Для 80% пользователей этого достаточно.

**Как:** Расширить JSON-ответ: добавить поле `"keywords": ["handmade silver ring", "boho jewelry", ...]`. Добавить блок в UI.

### TOP-4: Roast History (Effort: Medium, Impact: Medium)

**Что:** Сохранение предыдущих roasts в localStorage. Список: дата, платформа, score, первая строка текста. Кнопка "View" для каждого.

**Почему:** Позволяет трекить прогресс: "вчера было 4/10, сегодня после правок 7/10". Ни один конкурент не даёт историю оптимизации бесплатно. Это retention-фича.

**Как:** localStorage, массив объектов. UI: свёрнутая панель "History (3 roasts)" внизу страницы. Limit: 20 последних.

### TOP-5: Multi-format Export (Effort: Low, Impact: Medium)

**Что:** Кроме "Copy" -- кнопки "Export as TXT" и "Share as Image" (уже есть PNG). Добавить: "Copy for Etsy" (форматирование под Etsy), "Copy for Amazon" (bullet points).

**Почему:** Roast My Web берёт деньги за PDF-отчёты. Мы даём platform-specific export бесплатно. Это last-mile удобство.

**Как:** Форматтеры для каждой платформы. Чистый JS.

---

## 4. Что НЕ стоит делать в v2

| Фича | Почему нет |
|---|---|
| Фото-анализ | Требует vision API, сильно усложняет промпт, увеличивает стоимость BYOK |
| Реальные search volumes | Нужен бэкенд + API Etsy/Amazon. Противоречит архитектуре single-file |
| A/B тестирование | Нужен бэкенд для хранения вариантов и метрик |
| Competitor tracking | Нужен бэкенд + scraping |
| PDF-отчёты | Overkill для MVP, PNG share card достаточно |

---

## 5. Стратегическое позиционирование

```
+-----------------------------------------------------------+
|                    ДОРОГО ($30-359/мес)                    |
|                                                           |
|   Helium 10        Perci AI       PriceLabs              |
|   (Amazon all-in-1) (Amazon AI)   (Airbnb pricing)       |
|                                                           |
+-----------------------------------------------------------+
|                    СРЕДНЕ ($6-30/мес)                      |
|                                                           |
|   Marmalead    eRank Pro    EverBee    AutoRank           |
|   (Etsy SEO)  (Etsy audit) (Etsy res) (Airbnb opt)       |
|                                                           |
+-----------------------------------------------------------+
|                    БЕСПЛАТНО (BYOK)                        |
|                                                           |
|              >>> ROAST MY LISTING <<<                      |
|              Единственный бесплатный AI-критик             |
|              Multi-marketplace, instant, fun               |
|                                                           |
+-----------------------------------------------------------+
```

**Наша позиция:** бесплатный, мгновенный, мульти-платформенный AI-критик с юмором.

**Кто НЕ наш конкурент, а комплементарный инструмент:**
- Marmalead/eRank -- для deep SEO research (данные)
- Мы -- для instant copy feedback (текст)
- Связка: "Найди ключевики в eRank -> вставь объявление к нам -> получи roast -> скопируй rewrite"

---

## 6. Roadmap рекомендация: что внедрить для 9.5

Текущий score: **9.2/10** (audit_v2.md, все P1/P2 закрыты, 4 P3 осталось).

Для 9.5 нужно:

1. **Category-Specific Prompts** -- расширить system prompt для каждой платформы (LOW effort)
2. **Before/After Compare** -- side-by-side diff оригинала и rewrite (LOW effort)
3. **SEO Keywords block** -- 5-10 suggested keywords в результате (MEDIUM effort)

Эти три фичи:
- Закрывают gap с платными конкурентами (SEO + platform-specific)
- Дают уникальный UX (before/after diff)
- Не требуют бэкенда
- Не ломают BYOK-архитектуру
- Усиливают виральность (before/after карточка для share)

---

## Sources

- [Marmalead](https://marmalead.com/) -- Etsy SEO tool
- [Marmalead Review 2026](https://sellertoolshq.com/tools/marmalead/) -- features & pricing
- [eRank](https://erank.com/) -- Etsy analytics
- [eRank Plans & Pricing](https://public.erank.com/plans)
- [eRank Review 2026](https://www.listing-forge.com/blog/erank-review-2026)
- [EverBee](https://everbee.io/) -- Etsy product research
- [EverBee Pricing](https://everbee.io/pricing/)
- [EverBee Review 2026](https://sellertoolshub.com/tools/everbee)
- [Helium 10](https://www.helium10.com/) -- Amazon suite
- [Helium 10 Listing Analyzer](https://www.helium10.com/tools/listing-optimization/listing-analyzer/)
- [Helium 10 Pricing 2026](https://www.atom11.co/blog/helium-10-pricing)
- [Perci AI](https://www.perci.ai/) -- Amazon listing AI
- [PriceLabs](https://hello.pricelabs.co/) -- Airbnb dynamic pricing
- [PriceLabs Pricing](https://hello.pricelabs.co/plans/)
- [PriceLabs Listing Optimizer](https://hello.pricelabs.co/listing-optimizer/)
- [AutoRank](https://www.autorank.com/) -- Airbnb listing optimization
- [AutoRank Pricing](https://www.autorank.com/pricing)
- [Jurny Listing Optimizer](https://www.jurny.com/product/listing-optimizer)
- [Nifty AI](https://nifty.ai/) -- reseller crosslisting
- [Nifty Pricing](https://nifty.ai/pricing)
- [3Dsellers eBay AI](https://www.3dsellers.com/ebay-ai)
- [Roast My Web](https://www.roastmyweb.com/) -- website audit
- [Best Etsy SEO Tools 2026](https://www.growingyourcraft.com/blog/best-etsy-seo-tools-honest-review-free-options)
- [Amazon Listing Tools 2026](https://keywords.am/blog/best-amazon-listing-optimization-tools/)
- [AI Tools for Airbnb Hosts 2026](https://www.rakidzich.com/articles/airbnb-ai-tools-guide)
- [Airbnb Algorithm 2026 - NLP Search](https://www.nurturestays.ca/blog/how-airbnb-algorithm-works-ranking-guide-2026)
