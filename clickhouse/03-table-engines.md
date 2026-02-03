# ClickHouse Table Engines

## What is a Table Engine?
A table engine in ClickHouse defines:
- How data is stored
- How data is indexed
- How data is merged
- How queries are executed

👉 Choosing the right engine is critical for performance.

---

## MergeTree (Most Important)
- Base engine for most ClickHouse tables
- Supports:
  - Fast inserts
  - Partitioning
  - Sorting (ORDER BY)
  - Sparse indexing

### Key Features
- Data stored as immutable parts
- Background merge process
- Primary key used for data skipping

### When to Use
- Large datasets
- Analytical queries
- Time-series data

---

## ReplacingMergeTree
- Extension of MergeTree
- Removes duplicate rows during merge
- Uses a **version column** (optional)

### Use Case
- Deduplication
- Latest record per key

### Example
```sql
ENGINE = ReplacingMergeTree(updated_at)
