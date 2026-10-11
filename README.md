# NYC Taxi Time-Series Features and JSONB Event Log

## Dataset

- **Джерело:** NYC Yellow Taxi Trip Records, March 2024
- **Формат:** Parquet
- **URL:** https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-03.parquet
- **Отримання:** автоматичне завантаження через Python `urlretrieve`
- **Обсяг:** перші 500 000 рядків із подальшим очищенням даних.

## Features

### Time-Series ML Features

Таблиця `t7_hw_taxi_daily` містить денні агрегати поїздок за pickup zone.

- **Lag features:** кількість поїздок за попередній день, 7 і 30 попередніх спостережень.
- **Rolling average fare:** середній тариф за 7 календарних днів.
- **Rolling fare volatility:** стандартне відхилення тарифу за 30 календарних днів.
- **Cumulative features:** накопичена кількість і середня кількість поїздок.

Усі window functions використовують `PARTITION BY pickup_zone_id`.

### JSONB Event Log

Таблиця `t7_hw_events` містить 50 000 синтетичних подій п'яти типів: `page_view`, `click`, `add_to_cart`, `checkout` і `recommendation_shown`.

Використовуються JSONB-оператори `@>`, `?`, `?|`, JSONPath-функції, GIN-індекс і expression B-tree index. Продуктивність перевіряється через `EXPLAIN (ANALYZE, BUFFERS)` і порівнюється з нормалізованою checkout-таблицею.

## How to Run

1. Відкрити notebook у Google Colab.
2. Встановити залежності:

   ```python
   %pip install -q pgserver psycopg psycopg2-binary sqlalchemy pandas pyarrow requests
3. Запустити всі клітинки послідовно.
Notebook автоматично запускає PostgreSQL, завантажує та очищує дані, створює time-series features і JSONB event-log, виконує запити та зберігає результати в t7_hw_results. 
