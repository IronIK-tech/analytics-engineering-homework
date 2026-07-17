
# NBU Exchange Rates Data Pipeline

Practice building a data pipeline with the public NBU (National Bank of Ukraine) exchange rate API: full load, incremental load with deduplication, watermark-based incremental pipelines, upsert patterns, and logging/error handling.

# Topics Covered

- Fetching data from a public REST API (requests)
- Loading data into PostgreSQL with SQLAlchemy and pandas.to_sql
- Full load: TRUNCATE + append in a single transaction
- Incremental load by date with deduplication
- Watermark-based pipeline (resumable, tracks last processed date)
- Upsert pattern: staging table → INSERT ... ON CONFLICT DO UPDATE
- Logging (file + console handlers) and exception handling with logger.exception
