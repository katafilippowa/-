# Инвентаризационная матрица источников данных
## Группа Б | Анализ эффективности маркетинговых кампаний
**Целевая витрина:** `marketing_channel_effectiveness.csv`  

---

## 📋 Сводная таблица

| № | Домен | Атрибут (Поле) | Тип данных | Файл-источник | Система-источник | Владелец | Частота обновления | Правила ETL | Целевая витрина | Качество |
|---|-------|----------------|------------|---------------|------------------|----------|-------------------|-------------|-----------------|----------|
| 1 | Поддержка | ticket_id | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Проверка уникальности, удаление тестовых тикетов | marketing_channel_effectiveness | ✅ 100% заполнено, уникален |
| 2 | Поддержка | company_id | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Связь с marketing_leads, проверка на orphan records | marketing_channel_effectiveness | ✅ 100% заполнено |
| 3 | Поддержка | user_id | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Проверка на NULL, связь с CRM | marketing_channel_effectiveness | ⚠️ 85% заполнено |
| 4 | Поддержка | created_date | datetime | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Преобразование в ISO 8601, проверка часового пояса | marketing_channel_effectiveness | ✅ 100% заполнено |
| 5 | Поддержка | resolved_date | datetime | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Расчет response_time, обработка открытых тикетов (NULL) | marketing_channel_effectiveness | ⚠️ 90% заполнено (есть открытые) |
| 6 | Поддержка | priority | string | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Нормализация: [low, medium, high, urgent] → lowercase | marketing_channel_effectiveness | ✅ 98% заполнено |
| 7 | Поддержка | status | string | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Фильтрация: учитывать только закрытые для метрик времени | marketing_channel_effectiveness | ✅ 100% заполнено |
| 8 | Поддержка | category | string | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Нормализация категорий: техническая/биллинг/аккаунт | marketing_channel_effectiveness | ⚠️ 95% заполнено |
| 9 | Поддержка | satisfaction_score | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Фильтрация диапазона 1-5, исключение NULL из AVG | marketing_channel_effectiveness | ⚠️ 60% заполнено (не все оценивают) |
| 10 | Поддержка | response_time_min | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Расчет: (first_response - created) в минутах | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 11 | Поддержка | total_tickets | int | Расчетное | Агрегация | Аналитический отдел | Ежедневно | COUNT(DISTINCT ticket_id) по company_id | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 12 | Поддержка | avg_tickets_per_client | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | total_tickets / COUNT(DISTINCT company_id) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 13 | Поддержка | avg_satisfaction_score | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | AVG(satisfaction_score) WHERE score IS NOT NULL | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 14 | Поддержка | avg_response_time_min | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | AVG(resolved_date - created_date) для закрытых тикетов | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 15 | Маркетинг | lead_id | int | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Проверка уникальности, удаление дубликатов по lead_id | marketing_channel_effectiveness | ✅ 100% заполнено, уникален |
| 16 | Маркетинг | company_id | int | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Приведение к формату int, проверка на NULL | marketing_channel_effectiveness | ✅ 100% заполнено |
| 17 | Маркетинг | lead_date | date | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Преобразование в формат YYYY-MM-DD, проверка диапазона | marketing_channel_effectiveness | ✅ 100% заполнено |
| 18 | Маркетинг | channel | string | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Нормализация: lowercase, trim, маппинг в справочник каналов | marketing_channel_effectiveness | ⚠️ 98% заполнено, 2% требуют маппинга |
| 19 | Маркетинг | campaign_name | string | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Очистка от спецсимволов, замена NULL на 'Unknown' | marketing_channel_effectiveness | ⚠️ 95% заполнено |
| 20 | Маркетинг | campaign_cost | float | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Еженедельно | Замена NULL на 0, округление до 2 знаков после запятой | marketing_channel_effectiveness | ⚠️ 85% заполнено, 15% импутация |
| 21 | Маркетинг | lead_source | string | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Нормализация UTM source | marketing_channel_effectiveness | ⚠️ 90% заполнено |
| 22 | Маркетинг | conversion_flag | bool | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Расчет: TRUE если company_id есть в финансах с amount > 0 за 90 дней | marketing_channel_effectiveness | 🔄 Вычисляемое поле |
| 23 | Маркетинг | acquisition_month | date | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Расчет: DATE_TRUNC('month', lead_date) | marketing_channel_effectiveness | 🔄 Вычисляемое поле |
| 24 | Маркетинг | total_leads | int | Расчетное | Агрегация | Аналитический отдел | Еженедельно | COUNT(DISTINCT lead_id) по каналу+месяцу | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 25 | Маркетинг | converted_leads | int | Расчетное | Агрегация | Аналитический отдел | Еженедельно | COUNT с условием платежа за 90 дней | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 26 | Маркетинг | conversion_rate | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | converted_leads / NULLIF(total_leads, 0) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 27 | Маркетинг | total_campaign_cost | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | SUM(campaign_cost) по каналу+месяцу | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 28 | Маркетинг | cac | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | total_campaign_cost / NULLIF(converted_leads, 0) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 29 | Маркетинг | roi_percent | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | (revenue - cost) / NULLIF(cost, 0) × 100 | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 30 | Финансы | payment_id | int | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Проверка уникальности, автоинкремент | marketing_channel_effectiveness | ✅ 100% заполнено, уникален |
| 31 | Финансы | company_id | int | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Связь с marketing_leads по company_id, проверка на соответствие | marketing_channel_effectiveness | ✅ 100% заполнено |
| 32 | Финансы | doc_date | date | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Преобразование в YYYY-MM-DD, проверка на будущие даты | marketing_channel_effectiveness | ✅ 100% заполнено |
| 33 | Финансы | amount | float | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Отрицательные суммы → флаг возврата, округление до копеек | marketing_channel_effectiveness | ✅ 100% заполнено |
| 34 | Финансы | status | string | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Фильтрация: учитывать только status IN ('paid', 'completed') | marketing_channel_effectiveness | ✅ 99% заполнено |
| 35 | Финансы | total_revenue | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | SUM(amount) WHERE amount > 0 AND status != 'refunded' | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 36 | Финансы | avg_ltv | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | AVG по конвертированным клиентам, исключая выбросы > 99 перцентиля | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 37 | Финансы | churn_rate_90d | float | Расчетное | Агрегация | Аналитический отдел | Ежемесячно | 1 - (клиенты с повторным платежом за 90 дней / всего клиентов) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 38 | Справочник | product_id | int | 09_products.csv | Google Sheets | Продуктовый отдел | Еженедельно | Проверка уникальности, синхронизация с CRM | marketing_channel_effectiveness | ✅ 100% заполнено |
| 39 | Справочник | product_name | string | 09_products.csv | Google Sheets | Продуктовый отдел | Еженедельно | Очистка от дубликатов названий | marketing_channel_effectiveness | ✅ 100% заполнено |
| 40 | Справочник | product_category | string | 09_products.csv | Google Sheets | Продуктовый отдел | Еженедельно | Нормализация категорий продуктов | marketing_channel_effectiveness | ⚠️ 97% заполнено |
| 41 | Справочник | product_price | float | 09_products.csv | Google Sheets | Продуктовый отдел | Еженедельно | Проверка на отрицательные значения | marketing_channel_effectiveness | ✅ 100% заполнено |
| 42 | Справочник | is_active | bool | 09_products.csv | Google Sheets | Продуктовый отдел | Еженедельно | Фильтрация только активных продуктов | marketing_channel_effectiveness | ✅ 100% заполнено |
| 43 | Витрина | channel | string | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | Ключ группировки, нормализованный | marketing_channel_effectiveness | ✅ 100% заполнено |
| 44 | Витрина | acquisition_month | date | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | Ключ группировки по месяцам | marketing_channel_effectiveness | ✅ 100% заполнено |
| 45 | Витрина | quality_score | float | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | (avg_ltv / cac) × (1 / avg_tickets_per_client), нормализация 0-100 | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 46 | Витрина | data_updated_at | datetime | Система | marketing_channel_effectiveness | Аналитический отдел | При обновлении | CURRENT_TIMESTAMP при генерации витрины | marketing_channel_effectiveness | ✅ Автогенерация |
