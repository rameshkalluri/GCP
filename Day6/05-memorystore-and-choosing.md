# Memorystore & Choosing a Database

## Memorystore — the super-fast cache ⚡

**Memorystore** is a **fully managed in-memory data store** for **Redis** and
**Memcached** — used as a **cache** to make apps faster.

> **Analogy:** Instead of walking to the **big warehouse (database)** every time,
> you keep the **most-used items on your desk** for instant access. That desk is the
> cache — lightning fast, but small and temporary.

### Why cache?
- **Speed** — memory is far faster than disk/DB.
- **Reduce DB load** — serve repeat reads from cache.
- Common uses: **session storage, leaderboards, rate-limiting, hot lookups**.

### Redis vs Memcached (quick note)
- **Redis** — richer data types, persistence options, pub/sub. Most common choice.
- **Memcached** — simple, multi-threaded key/value cache.

> A cache is **not your source of truth** — it can be cleared anytime. Keep the real
> data in a database.

```bash
gcloud redis instances create my-cache \
  --size=1 --region=asia-south1 --redis-version=redis_7_0
```

---

## Choosing a GCP Database — decision guide 🧭

```
Is the data structured records you query with SQL?
   ├── YES → Need GLOBAL scale + strong consistency?
   │          ├── YES → Cloud Spanner
   │          └── NO  → Cloud SQL (MySQL/Postgres/SQL Server)
   │
   └── NO (NoSQL) → What's the workload?
              ├── App data, flexible docs, real-time/mobile → Firestore
              ├── Huge writes, time-series/IoT, petabyte     → Bigtable
              └── Just need a fast cache in front of a DB     → Memorystore

Need heavy ANALYTICS over huge datasets (OLAP)?  → BigQuery (Day 9)
```

---

## Cheat-Sheet Table

| Need | Service | One-liner |
|---|---|---|
| Relational app DB | **Cloud SQL** | Managed MySQL/Postgres/SQL Server |
| Global relational + strong consistency | **Cloud Spanner** | SQL at planet scale |
| Flexible app/mobile data, real-time | **Firestore** | Serverless NoSQL documents |
| Massive NoSQL, time-series/IoT | **Bigtable** | Petabyte wide-column, high throughput |
| Speed up reads / caching | **Memorystore** | In-memory Redis/Memcached |
| Big analytics (OLAP) | **BigQuery** | Serverless data warehouse (Day 9) |

---

## Managed vs Self-Managed reminder
- **Prefer managed** services (less ops, built-in HA/backups).
- Self-manage a DB on a VM only for special needs (custom extensions, licensing, full control).

---

## TL;DR (in plain English)
- **Memorystore = in-memory cache** (Redis/Memcached) — keep hot data "on your desk"
  for speed; it's not the source of truth.
- **Pick by data shape + scale:** Cloud SQL (relational), Spanner (global SQL),
  Firestore (flexible NoSQL/real-time), Bigtable (massive NoSQL), Memorystore (cache),
  BigQuery (analytics).
- When in doubt: relational + typical → **Cloud SQL**; flexible app data → **Firestore**.
