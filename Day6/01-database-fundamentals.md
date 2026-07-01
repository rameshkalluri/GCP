# Database Fundamentals

Before picking a GCP database, understand the big choices: **SQL vs NoSQL**,
**managed vs self-managed**, and **OLTP vs OLAP**.

---

## What is a Database?
A **database** stores **structured, queryable data** — records you can search, filter,
sort, and update efficiently.

> **Analogy:** A bucket/disk is a **filing cabinet** (store & fetch whole files). A
> database is a **smart librarian** who instantly answers precise questions like "all
> customers in Mumbai who bought in June."

---

## SQL vs NoSQL

| | **SQL (Relational)** | **NoSQL (Non-relational)** |
|---|---|---|
| Data shape | **Tables** with rows/columns, fixed schema | Flexible: **documents, key-value, wide-column, graph** |
| Query language | **SQL** | Varies (APIs, limited queries) |
| Best for | Structured data, relationships, transactions | Huge scale, flexible/changing data, high write throughput |
| Consistency | Strong (ACID) | Often eventual (tunable) |
| Analogy | A strict **accountant's ledger** (rules, columns) | A box of **sticky notes** (each can look different) |
| GCP examples | Cloud SQL, Spanner | Firestore, Bigtable |

> **ACID** (Atomicity, Consistency, Isolation, Durability) = the promise that money
> moving between accounts either **fully happens or not at all** — critical for banking.

---

## Managed vs Self-Managed

| | **Self-managed (DB on a VM)** | **Managed (Cloud SQL, etc.)** |
|---|---|---|
| You handle | OS, patching, backups, replication, scaling | Just your data & queries |
| Provider handles | Nothing (it's your VM) | Patching, backups, HA, failover |
| Analogy | **Cooking at home** (full control, all the work) | **Restaurant** (you just order & eat) |
| Trade-off | Max control, max effort | Less control, far less ops burden |

> Rule of thumb: **prefer managed** unless you have a specific reason (custom extensions,
> licensing, full control) to run it yourself.

---

## OLTP vs OLAP (two very different jobs)

| | **OLTP** (Transactional) | **OLAP** (Analytical) |
|---|---|---|
| Purpose | Day-to-day operations (orders, logins) | Analytics over huge datasets |
| Pattern | Many small, fast reads/writes | Few big, complex scans/aggregations |
| Analogy | The **cash register** at a shop | The **year-end business report** |
| GCP fit | Cloud SQL, Firestore, Spanner | **BigQuery** (Day 9), Bigtable (for some) |

> Don't run heavy analytics on your OLTP database — it slows down the cash register.
> Move analytics to **BigQuery**.

---

## CAP Theorem (one-liner)
In a distributed database, during a network partition you can favor **Consistency** or
**Availability**, not both perfectly. Spanner is famous for offering strong consistency
at global scale (using Google's TrueTime).

---

## TL;DR (in plain English)
- A **database = a smart librarian** for structured, queryable records.
- **SQL** = strict tables + relationships + ACID; **NoSQL** = flexible + massive scale.
- **Managed** (Cloud SQL, etc.) = Google handles ops; **self-managed** = you do it all on a VM.
- **OLTP** = fast day-to-day transactions; **OLAP** = big analytics (use **BigQuery**).
- Prefer **managed** services and match the **data shape** to the right database.
