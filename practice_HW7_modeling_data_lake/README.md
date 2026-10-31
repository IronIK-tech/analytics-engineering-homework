# Practice: Data Lake Modeling

A data pipeline for a coffee shop chain built in **Azure Blob Storage** using the **medallion architecture**: raw files land in the Bronze layer, get cleaned in Silver, and are modeled into a star schema in Gold.

## Tools

- Python 3.13, Jupyter Notebook (PyCharm)
- pandas, pyarrow
- azure-storage-blob (`ContainerClient` + SAS tokens)

## Source data

| File | Description | Rows |
|---|---|---|
| `orders_2026_01..03.json` | receipt line items, January–March | 300 + 340 + 340 |
| `shops.json` | coffee shop reference data | 5 |
| `products.json` | menu | 12 |

## Layers

**Bronze** holds raw data as-is. The three order files are combined with `pd.concat` (980 rows); reference tables are stored unchanged.

**Silver** holds cleaned data and reads from Bronze only:
- removed 20 full duplicate rows (980 → 960);
- normalized `shop_code` (`strip` + `upper`): 10 variants → 5;
- normalized `payment_type` to `Card` / `Cash`: 4 variants → 2;
- filled 36 empty `qty` values with 1 and cast the column to `int`;
- converted `order_ts` to `datetime` and added `order_date`;
- added `amount = qty × unit_price`.

**Gold** holds the star schema and reads from
