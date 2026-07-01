# BigQuery & Data

## What is BigQuery?
**BigQuery** is Google's **serverless data warehouse** for **analytics (OLAP)** — run
**SQL** over **terabytes to petabytes** in seconds, with no servers to manage.

> **Analogy:** If Cloud SQL is a **cash register** (fast small transactions), BigQuery
> is the **giant research library + army of analysts** that can scan **millions of
> records** and answer a big question ("total sales per region last year") in seconds.

---

## Why BigQuery is special
- **Serverless** — no infrastructure; it scales compute automatically.
- **Massive scale** — analyze petabytes with standard **SQL**.
- **Separation of storage & compute** — pay for each independently.
- **Fast** — distributes queries across thousands of machines.
- **Built-in ML** (**BigQuery ML**) — train models with SQL.
- **Public datasets** — practice on free real-world data.

---

## OLTP vs OLAP reminder (where BigQuery fits)
| | **OLTP** (Cloud SQL, Firestore) | **OLAP** (BigQuery) |
|---|---|---|
| Job | Fast day-to-day transactions | Big analytical queries |
| Pattern | Many small reads/writes | Few huge scans/aggregations |
| Analogy | Cash register | Year-end business report |

> Don't run heavy analytics on your app database — **export/stream to BigQuery** and
> analyze there.

---

## Pricing model (be careful!)
- **Storage** — priced per GB stored.
- **Query** — **on-demand** priced by **bytes scanned** (or flat-rate slots).
- 💡 **Select only needed columns** (it's columnar) and use **partitioning/clustering**
  to scan less data = **cheaper + faster**.
- *Analogy:* you pay by **how many shelves the analysts walk past** — don't send them
  through the whole library for one book.

```sql
-- Query a public dataset
SELECT name, SUM(number) AS total
FROM `bigquery-public-data.usa_names.usa_1910_2013`
WHERE state = 'CA'
GROUP BY name
ORDER BY total DESC
LIMIT 10;
```

```bash
bq query --use_legacy_sql=false 'SELECT 1'
```

---

## The data pipeline friends 🔗

### Pub/Sub — the messaging post office 📬
- A **messaging service** to decouple systems: publishers send messages to a **topic**;
  subscribers receive them.
- *Analogy:* a **post office** — senders drop letters (messages); receivers pick them up
  independently, even at different speeds.
- Great for **event ingestion**, streaming, and connecting microservices.

### Dataflow — the conveyor belt 🏭
- Managed **stream & batch data processing** (based on Apache Beam) to transform/clean
  data as it moves (e.g., Pub/Sub → Dataflow → BigQuery).
- *Analogy:* a **factory conveyor belt** that cleans, sorts, and packages data on the way through.

### Typical analytics pipeline
```
Sources → Pub/Sub (ingest) → Dataflow (transform) → BigQuery (analyze) → Dashboards (Looker)
```

---

## TL;DR (in plain English)
- **BigQuery = serverless data warehouse** for big **analytics (OLAP)** with SQL —
  petabytes in seconds, no servers.
- It's for **analysis**, not transactions (use Cloud SQL/Firestore for OLTP); watch cost
  by **scanning fewer bytes** (select columns, partition/cluster).
- Data pipeline friends: **Pub/Sub** (messaging/ingest) and **Dataflow** (stream/batch
  processing): *Sources → Pub/Sub → Dataflow → BigQuery.*
