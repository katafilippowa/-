# Проект витрины данных: marketing_channel_effectiveness.csv

## 1. Бизнес-контекст
**Задача:** Анализ эффективности маркетинговых кампаний  
**Вопрос:** Какие каналы привлечения приносят наиболее качественных клиентов?  
**Критерии качества:** Высокий LTV (общая сумма покупок), низкий отток, низкая нагрузка на поддержку.

## 2. Уровень агрегации
`Канал + Кампания + Когортный месяц` (месяц первого успешного платежа клиента)  
Одна строка витрины = все клиенты, привлеченные через конкретный канал/кампанию в конкретный месяц.

## 3. Структура витрины

| Поле | Тип | Источник | Описание | Способ расчета |
|------|-----|----------|----------|----------------|
| `channel_id` | string | 07_marketing_leads | Канал привлечения | Прямое поле `channel` |
| `campaign_id` | string | 07_marketing_leads | Идентификатор кампании | Прямое поле `campaign` |
| `cohort_month` | date | 07/06 + маппинг | Месяц первой оплаты клиента | `DATE_TRUNC('month', first_payment_date)` |
| `leads_count` | int | 07_marketing_leads | Количество привлеченных лидов | `COUNT(DISTINCT lead_id)` |
| `converted_customers` | int | 07 + маппинг | Количество клиентов, совершивших покупку | `COUNT(DISTINCT company_id)` |
| `conversion_rate` | float | Расчетное | Доля лидов, ставших клиентами | `converted_customers / leads_count * 100` |
| `ltv_avg` | decimal | 07 + маппинг | Средняя пожизненная ценность клиента | `SUM(amount) / converted_customers` |
| `churn_rate_90d` | float | 06 + маппинг | Доля оттока через 90 дней | `churned_customers / converted_customers` |
| `support_tickets_avg` | float | 06_support_tickets | Среднее кол-во тикетов на клиента | `COUNT(ticket_id) / converted_customers` |
| `support_rating_avg` | float | 06_support_tickets | Средняя оценка качества поддержки | `AVG(rating)` где `rating IS NOT NULL` |
| `product_category_top` | string | 09_products | Наиболее востребованная категория | `MODE(category)` по когорте |
| `channel_quality_score` | float | Расчетное | Композитная метрика эффективности | `(ltv_norm * 0.5) + ((1-churn) * 0.3) + ((1-tickets_norm) * 0.2)` |


## 4. Потребители витрины
- **Маркетинг-департамент** — оптимизация рекламного бюджета
- **Руководство холдинга** — оценка ROI каналов
- **Аналитический департамент** — прогнозирование и сегментация
