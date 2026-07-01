# Cloud SQL

## What is Cloud SQL?
**Cloud SQL** is Google's **fully managed relational database** service for **MySQL**,
**PostgreSQL**, and **SQL Server**.

> **Analogy:** It's like hiring a **restaurant kitchen** instead of cooking at home.
> You still choose the dish (MySQL/Postgres), but Google does the **patching, backups,
> and repairs** — you just store and query your data.

---

## What Google manages for you
- **Provisioning** the database server.
- **Patching & updates** (OS + DB engine).
- **Automated backups** + point-in-time recovery.
- **High availability** (optional) with automatic failover.
- **Read replicas** for scaling reads.
- **Encryption** at rest and in transit (default).

You focus on **schemas, queries, and data** — not servers.

---

## Key features

### High Availability (HA)
- Runs a **primary + standby** in **two zones** in the same region.
- If the primary's zone fails, it **auto-fails over** to the standby.
- *Analogy:* a **backup chef** ready to take over instantly if the main chef is out.

### Read Replicas
- Extra **read-only** copies to handle heavy read traffic (reports, dashboards).
- *Analogy:* extra **photocopies of the ledger** so many people can read at once —
  but only the original can be **written** to.

### Backups & Recovery
- **Automated daily backups** + **point-in-time recovery (PITR)** to undo mistakes.
- *Analogy:* a **time machine** to rewind the database to just before the bad change.

---

## When to use Cloud SQL ✅
- Traditional apps needing a **relational** DB (users, orders, inventory).
- You want **MySQL/Postgres/SQL Server** without managing servers.
- Data fits comfortably on **one regional** database (up to tens of TB).

## When NOT to use it ❌
- You need **global**, horizontally-scalable SQL → use **Spanner**.
- You need **massive NoSQL scale** (huge writes) → **Bigtable/Firestore**.
- Heavy **analytics** over billions of rows → **BigQuery**.

---

## Cloud SQL vs Spanner (quick contrast)
| | **Cloud SQL** | **Cloud Spanner** |
|---|---|---|
| Scale | Single regional server (+ replicas) | Horizontally scalable, **global** |
| SQL | Yes (MySQL/Postgres/SQL Server) | Yes (Google SQL dialect) |
| Cost | Lower | Higher (premium) |
| Use when | Most typical apps | Global, huge, always-on relational needs |

---

## Quick `gcloud` Reference
```bash
# Create a MySQL instance
gcloud sql instances create my-db \
  --database-version=MYSQL_8_0 \
  --tier=db-f1-micro --region=asia-south1

# Set the root password
gcloud sql users set-password root --host=% \
  --instance=my-db --password=SECRET

# Create a database
gcloud sql databases create appdb --instance=my-db

# Connect from Cloud Shell
gcloud sql connect my-db --user=root

# Create a read replica
gcloud sql instances create my-db-replica \
  --master-instance-name=my-db --region=asia-south1
```

---

## TL;DR (in plain English)
- **Cloud SQL = managed MySQL/PostgreSQL/SQL Server** — Google handles patching,
  backups, HA, and failover.
- **HA** = primary + standby across zones with auto-failover; **read replicas** scale reads.
- Great for **typical relational apps**; not for global-scale SQL (**Spanner**),
  massive NoSQL (**Bigtable/Firestore**), or big analytics (**BigQuery**).
