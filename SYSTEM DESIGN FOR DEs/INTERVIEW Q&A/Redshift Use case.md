# Redshift Data Warehouse Schema for E-commerce

## Schema Design: Star Schema

### Fact Table
**`fact_orders`**
- **Grain**: One row per order line item
- **Columns**:
  - `order_id`
  - `customer_id`
  - `product_id`
  - `order_date_id`
  - `quantity`
  - `unit_price`
  - `discount`
  - `total_amount`

Captures daily order volumes and revenue metrics.

### Dimension Tables

**`dim_customer`**
- Customer details (name, region, signup date, etc.)
- **SCD Type 2**: Tracks changing attributes like address or loyalty tier

**`dim_product`**
- Product details (product_id, category, brand, etc.)
- **SCD Type 2**: Tracks category/brand reassignments

**`dim_date`**
- Calendar details (day, week, month, quarter, year)

**`dim_order_status`**
- Status lifecycle (pending, shipped, delivered, returned)

## Data Pipeline

### CDC Handling
- **New orders**: Ingest incrementally via CDC from source (daily)
- **Late-arriving data**: Load into staging, then upsert into `fact_orders`

## Redshift Optimizations

- **DISTKEY**: `customer_id` (optimizes queries joining customer → orders)
- **SORTKEY**: `order_date` (optimizes time-based queries)
- **Compression**: Apply column encodings via `ANALYZE COMPRESSION`

## Supported Analytics

- 📈 **Daily sales trends**: `SUM(total_amount) by date`
- 🏷️ **Revenue per category**: JOIN `fact_orders` → `dim_product`
- 🔄 **Repeat customer analysis**: JOIN `fact_orders` → `dim_customer`
