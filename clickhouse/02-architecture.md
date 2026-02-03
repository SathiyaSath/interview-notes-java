# ClickHouse Architecture

## High-level Architecture
ClickHouse follows a **distributed, column-oriented OLAP architecture** designed for fast analytical queries.

Main components:
- Client
- Server
- Storage Engine
- Distributed Cluster (optional)

---

## Column-Oriented Storage
- Data is stored **column by column**, not row by row
- Only required columns are read during queries
- Results in:
  - Less disk I/O
  - Better compression
  - Faster aggregation queries

👉 Reason ClickHouse is faster than MySQL for analytics.

---

## ClickHouse Server
- Handles query parsing, execution, and optimization
- Uses **vectorized execution** (processes data in batches)
- Executes queries in parallel using multiple CPU cores

---

## Storage Layer
- Data is stored in **tables** using different **table engines**
- Most common engine: **MergeTree**
- Data is written as immutable **parts**
- Background process merges parts automatically

---

## MergeTree Architecture (Core Concept)
Each table consists of:
- Parts (small immutable data files)
- Partitions (logical grouping, e.g. by date)
- Primary key (used for sparse indexing)
- ORDER BY (defines data sorting on disk)

👉 Inserts are fast because data is written as new parts.

---

## Sparse Indexing
- ClickHouse uses **sparse indexes**, not row-level indexes
- Index stores min/max values per data block
- During query:
  - Blocks not matching condition are skipped
  - Reduces disk scan significantly

---

## Query Execution Flow
1. Client sends SQL query
2. Server parses & optimizes query
3. Required columns are read from disk
4. Indexes skip unnecessary data
5. Data processed in parallel
6. Result returned to client

---

## Distributed Architecture (Optional)
- ClickHouse supports clusters with multiple nodes
- Uses:
  - Shards (data distribution)
  - Replicas (fault tolerance)
- `Distributed` table engine is used

---

## Why ClickHouse is Not Good for OLTP
- Updates & deletes are expensive
- No row-level locking
- Best for:
  - Reporting
  - Dashboards
  - Logs & metrics
  - Analytics

---

## Interview One-Line Summary
"ClickHouse uses columnar storage, sparse indexing, and MergeTree architecture to deliver high-performance analytical queries on large datasets."
