# Bigtable & Spanner

Two "big-scale" databases for special jobs: **Bigtable** (massive NoSQL) and
**Spanner** (global SQL).

---

## Cloud Bigtable — massive NoSQL wide-column 📊

**Bigtable** is a **fully managed, petabyte-scale NoSQL** database built for **huge
volumes** of data and **very high read/write throughput** with low latency.

> **Analogy:** Imagine an **enormous spreadsheet with billions of rows** that never
> slows down — designed for firehoses of data like sensor readings or clickstreams.

### Key facts
- **Wide-column** store; every row has a single **row key** (choose it carefully!).
- Blazing fast for **time-series, IoT, financial ticks, ad-tech, monitoring**.
- Scales by **adding nodes**; no SQL, no joins.
- Powers Google products (Search, Maps, Analytics) internally.

### Use Bigtable when ✅
- Billions of rows / terabytes–petabytes.
- Very high **write** throughput + low-latency lookups by key.
- Time-series / IoT / analytics workloads.

### Avoid when ❌
- You need transactions, joins, or SQL → **Cloud SQL/Spanner**.
- Small datasets (it's overkill and pricier).

---

## Cloud Spanner — global, strongly-consistent SQL 🌍

**Spanner** combines the **structure of SQL** with the **horizontal scale of NoSQL**,
adding **global distribution** and **strong consistency**.

> **Analogy:** A ledger that is **strict and correct like a bank's**, but also
> **stretched across the whole planet** — everyone everywhere sees the same, correct
> balance at the same time.

### Key facts
- **Relational + SQL**, but **horizontally scalable** and **multi-region**.
- **Strong consistency** globally using Google's **TrueTime**.
- **99.999%** availability (multi-region) — extremely high.
- Premium price — use when you truly need global scale + consistency.

### Use Spanner when ✅
- Global apps needing **consistent** relational data (banking, inventory, gaming).
- You've outgrown a single Cloud SQL instance and need to scale writes horizontally.

### Avoid when ❌
- Small/single-region app → **Cloud SQL** is far cheaper.
- Non-relational, flexible data → **Firestore/Bigtable**.

---

## Bigtable vs Spanner vs Cloud SQL (at a glance)

| | **Cloud SQL** | **Bigtable** | **Spanner** |
|---|---|---|---|
| Type | Relational (SQL) | NoSQL wide-column | Relational (SQL) |
| Scale | Single region (+replicas) | Petabyte, high throughput | Horizontal, **global** |
| Consistency | Strong | Eventual/row-level | **Strong, global** |
| Transactions/joins | Yes | No | Yes |
| Best for | Typical apps | IoT/time-series/analytics | Global critical apps |
| Cost | $ | $$ | $$$ |

---

## Quick `gcloud` Reference
```bash
# Bigtable instance
gcloud bigtable instances create my-bt \
  --display-name="My Bigtable" \
  --cluster-config=id=c1,zone=asia-south1-a,nodes=1

# Spanner instance + database
gcloud spanner instances create my-spanner \
  --config=regional-asia-south1 --nodes=1 --description="My Spanner"
gcloud spanner databases create appdb --instance=my-spanner
```

---

## TL;DR (in plain English)
- **Bigtable = massive NoSQL** for firehoses of data (IoT, time-series, analytics) —
  billions of rows, no SQL/joins, pick the **row key** wisely.
- **Spanner = global SQL** — relational + horizontally scalable + **strongly consistent
  worldwide** (premium price).
- Use **Cloud SQL** for typical apps, **Bigtable** for scale/throughput, **Spanner**
  for global consistency.
