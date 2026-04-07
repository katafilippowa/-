# Инвентаризационная матрица источников данных
## Группа Б | Анализ эффективности маркетинговых кампаний


---

## 📋 Сводная таблица

| № | Домен | Атрибут (Поле) | Тип данных | Файл-источник | Система-источник | Владелец | Частота обновления | Правила ETL | Целевая витрина | Качество |
|---|-------|----------------|------------|---------------|------------------|----------|-------------------|-------------|-----------------|----------|
| 1 | Маркетинг | lead_id | int | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Проверка уникальности, удаление дубликатов по lead_id | marketing_channel_effectiveness | ✅ 100% заполнено, уникален |
| 2 | Маркетинг | company_id | int | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Приведение к формату int, проверка на NULL | marketing_channel_effectiveness | ✅ 100% заполнено |
| 3 | Маркетинг | channel | string | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Нормализация: lowercase, trim, маппинг в справочник каналов | marketing_channel_effectiveness | ⚠️ 98% заполнено, 2% требуют маппинга |
| 4 | Маркетинг | campaign_name | string | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Очистка от спецсимволов, замена NULL на 'Unknown' | marketing_channel_effectiveness | ⚠️ 95% заполнено |
| 5 | Маркетинг | lead_date | date | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Преобразование в формат YYYY-MM-DD, проверка диапазона | marketing_channel_effectiveness | ✅ 100% заполнено |
| 6 | Маркетинг | campaign_cost | float | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Еженедельно | Замена NULL на 0, округление до 2 знаков после запятой | marketing_channel_effectiveness | ⚠️ 85% заполнено, 15% импутация |
| 7 | Маркетинг | conversion_flag | bool | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Расчет: TRUE если company_id есть в финансах с amount > 0 за 90 дней | marketing_channel_effectiveness | 🔄 Вычисляемое поле |
| 8 | Маркетинг | acquisition_month | date | 07_marketing_leads.csv | Маркетинговая платформа | Маркетинговый отдел | Ежедневно | Расчет: DATE_TRUNC('month', lead_date) | marketing_channel_effectiveness | 🔄 Вычисляемое поле |
| 9 | Финансы | payment_id | int | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Проверка уникальности, автоинкремент | marketing_channel_effectiveness | ✅ 100% заполнено, уникален |
| 10 | Финансы | company_id | int | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Связь с marketing_leads по company_id, проверка на соответствие | marketing_channel_effectiveness | ✅ 100% заполнено |
| 11 | Финансы | doc_date | date | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Преобразование в YYYY-MM-DD, проверка на будущие даты | marketing_channel_effectiveness | ✅ 100% заполнено |
| 12 | Финансы | amount | float | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Отрицательные суммы → флаг возврата, округление до копеек | marketing_channel_effectiveness | ✅ 100% заполнено |
| 13 | Финансы | payment_type | string | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Приведение к справочнику: [card, transfer, invoice, other] | marketing_channel_effectiveness | ⚠️ 97% заполнено, 3% маппинг |
| 14 | Финансы | status | string | 05_finance_payments.csv | 1С | Финансовый отдел | Ежедневно | Фильтрация: учитывать только status IN ('paid', 'completed') | marketing_channel_effectiveness | ✅ 99% заполнено |
| 15 | Финансы | total_revenue | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | SUM(amount) WHERE amount > 0 AND status != 'refunded' | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 16 | Финансы | avg_ltv | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | AVG по конвертированным клиентам, исключая выбросы > 99 перцентиля | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 17 | Финансы | churn_rate_90d | float | Расчетное | Агрегация | Аналитический отдел | Ежемесячно | 1 - (клиенты с повторным платежом за 90 дней / всего клиентов) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 18 | Поддержка | ticket_id | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Проверка уникальности, удаление тестовых тикетов | marketing_channel_effectiveness | ✅ 100% заполнено, уникален |
| 19 | Поддержка | company_id | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Связь с marketing_leads, проверка на orphan records | marketing_channel_effectiveness | ✅ 100% заполнено |
| 20 | Поддержка | created_date | datetime | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Преобразование в ISO 8601, проверка часового пояса | marketing_channel_effectiveness | ✅ 100% заполнено |
| 21 | Поддержка | resolved_date | datetime | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Расчет response_time, обработка открытых тикетов (NULL) | marketing_channel_effectiveness | ⚠️ 90% заполнено (есть открытые) |
| 22 | Поддержка | satisfaction_score | int | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Фильтрация диапазона 1-5, исключение NULL из AVG | marketing_channel_effectiveness | ⚠️ 60% заполнено (не все оценивают) |
| 23 | Поддержка | priority | string | 06_support_tickets.csv | Zendesk | Служба поддержки | В реальном времени | Нормализация: [low, medium, high, urgent] → lowercase | marketing_channel_effectiveness | ✅ 98% заполнено |
| 24 | Поддержка | total_tickets | int | Расчетное | Агрегация | Аналитический отдел | Ежедневно | COUNT(DISTINCT ticket_id) по company_id | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 25 | Поддержка | avg_tickets_per_client | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | total_tickets / COUNT(DISTINCT company_id) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 26 | Поддержка | avg_response_time_min | float | Расчетное | Агрегация | Аналитический отдел | Еженедельно | AVG(resolved_date - created_date) в минутах, только для закрытых | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 27 | HR | employee_id | int | 08_hr_employees.csv | HR-система | HR-отдел | По изменениям | Фильтрация: только is_active = TRUE | Справочник владельцев | ✅ 100% заполнено |
| 28 | HR | department | string | 08_hr_employees.csv | HR-система | HR-отдел | По изменениям | Нормализация названий департаментов | Справочник владельцев | ✅ 100% заполнено |
| 29 | HR | position | string | 08_hr_employees.csv | HR-система | HR-отдел | По изменениям | Очистка от дублирующих должностей | Справочник владельцев | ⚠️ 98% заполнено |
| 30 | Витрина | channel | string | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | Ключ группировки, нормализованный | marketing_channel_effectiveness | ✅ 100% заполнено |
| 31 | Витрина | acquisition_month | date | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | Ключ группировки по месяцам | marketing_channel_effectiveness | ✅ 100% заполнено |
| 32 | Витрина | total_leads | int | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | COUNT(DISTINCT lead_id) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 33 | Витрина | converted_leads | int | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | COUNT с условием платежа за 90 дней | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 34 | Витрина | conversion_rate | float | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | converted_leads / NULLIF(total_leads, 0) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 35 | Витрина | cac | float | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | total_campaign_cost / NULLIF(converted_leads, 0) | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 36 | Витрина | roi_percent | float | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | (revenue - cost) / NULLIF(cost, 0) × 100 | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 37 | Витрина | quality_score | float | Агрегация | marketing_channel_effectiveness | Аналитический отдел | Еженедельно | (avg_ltv / cac) × (1 / avg_tickets_per_client), нормализация 0-100 | marketing_channel_effectiveness | 🔄 Расчетное поле |
| 38 | Витрина | data_updated_at | datetime | Система | marketing_channel_effectiveness | Аналитический отдел | При обновлении | CURRENT_TIMESTAMP при генерации витрины | marketing_channel_effectiveness | ✅ Автогенерация |

