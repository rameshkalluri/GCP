# Choosing the Right Storage

With several storage options, the trick is matching the **data** to the **right
container**. Here's a simple guide.

> **Analogy:** You wouldn't keep ice cream in the garage or park your car in the fridge.
> Each kind of "stuff" has a **natural home**. Same with cloud data.

---

## The Big 3 (and their natural homes)

| Need | Use | Analogy |
|---|---|---|
| Store **files** fetched over the internet (images, backups, video, datasets) | **Cloud Storage (bucket)** | **Warehouse** of boxes |
| A **single VM's** hard drive (OS, app data) | **Persistent Disk** | The **hard drive in your PC** |
| A **shared folder** for **many VMs** | **Filestore** | The **office shared drive** |

---

## Quick Decision Flow

```
Is it a file you access over the web / store in bulk (backups, media, big data)?
   └── YES → Cloud Storage (bucket)
              └── How often will you read it?
                    Often   → Standard
                    Monthly → Nearline
                    Quarterly → Coldline
                    Almost never (archive) → Archive

Does a VM need a drive to boot/run from?
   └── YES → Persistent Disk
              └── Need top speed? → SSD/Extreme   |  Cheap & bulky? → Standard (HDD)
              └── Need ultra-fast & OK to lose on stop? → Local SSD

Do MANY VMs need to share the same files at once?
   └── YES → Filestore (shared NFS)
```

---

## Databases are different! 🗄️
If your data is **structured records you query** (users, orders, transactions), you
usually want a **database**, not raw storage.

> **Analogy:** A bucket/disk is a **filing cabinet** (you store and fetch whole files).
> A database is a **smart librarian** who can instantly answer "give me all orders over
> $100 from June." That's **Day 6** (Cloud SQL, Firestore, etc.).

| Data shape | Use |
|---|---|
| Whole files / blobs | Cloud Storage |
| A VM's disk | Persistent Disk |
| Shared files for many VMs | Filestore |
| Structured/queryable records | **A database (Day 6)** |

---

## Cost Cheat-Sheet
- Buckets: pick the **right class** + **lifecycle rules** to auto-cool/delete old data.
- Disks: **right-size**, delete unused disks/snapshots, prefer Balanced unless you need SSD.
- Filestore: priced by **provisioned capacity** — don't over-provision.
- *Analogy:* don't rent a **whole warehouse** for one box, or a **race-car drive** for
  files you open once a year.

---

## TL;DR (in plain English)
- **Files over the web / bulk** → **Cloud Storage** (then pick a class by access frequency).
- **One VM's drive** → **Persistent Disk** (SSD for speed, HDD for cheap/bulk; Local SSD = fast but temporary).
- **Many VMs share files** → **Filestore**.
- **Structured, queryable data** → a **database (Day 6)**, not raw storage.
- Match the **data type** to its **natural home**, and use **classes/lifecycle/right-sizing** to save money.
