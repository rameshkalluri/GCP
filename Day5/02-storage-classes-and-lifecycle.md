# Storage Classes & Lifecycle

## Storage Classes = Hot vs Cold Shelves 🔥🧊

Not all files are used equally. Some you open daily; some you keep "just in case" for
years. **Storage classes** let you pay less for files you rarely touch.

> **Analogy:** Think of your storage as shelves:
> - **Hot shelf** (front of the room) — easy to grab, but rent is higher.
> - **Cold shelf / deep basement** — cheap to keep, but slow & costly to fetch.
> You put **daily stuff up front** and **old stuff in the basement**.

---

## The 4 Storage Classes

| Class | Best for | Min storage time | Analogy |
|---|---|---|---|
| **Standard** | Frequently used data ("hot") | None | Items on your **desk** — grab anytime |
| **Nearline** | Accessed ~once a month | 30 days | **Hallway closet** — occasional use |
| **Coldline** | Accessed ~once a quarter | 90 days | **Garage** — rarely opened |
| **Archive** | Rarely ever (backups, compliance) | 365 days | **Deep basement vault** — almost never touched |

### The trade-off (important!)
- **Colder class = cheaper to store, but...**
  - more expensive to **retrieve/read**, and
  - has a **minimum storage duration** (delete early = you still pay for the minimum).
- *Analogy:* the **basement vault** is cheap rent, but every time you go down to grab
  something, there's a **fetch fee**, and you've **rented the spot for a minimum term**.

> Same speed of access for all classes — the difference is **price**, not performance.

---

## Lifecycle Rules = Auto-Tidying 🤖🧹

A **lifecycle rule** automatically moves or deletes objects based on age/conditions —
so you don't have to clean up by hand.

> **Analogy:** A **robot helper** that follows house rules:
> - "If a file is **older than 30 days**, move it to the **closet** (Nearline)."
> - "If older than **1 year**, move it to the **basement** (Archive)."
> - "If older than **3 years**, **throw it out** (delete)."

### Common rules
- **Downgrade class** as data ages (Standard → Nearline → Coldline → Archive).
- **Delete** objects after N days.
- **Delete old versions** (if object versioning is on).

### Example lifecycle policy (JSON)
```json
{
  "rule": [
    { "action": {"type": "SetStorageClass", "storageClass": "NEARLINE"},
      "condition": {"age": 30} },
    { "action": {"type": "SetStorageClass", "storageClass": "ARCHIVE"},
      "condition": {"age": 365} },
    { "action": {"type": "Delete"},
      "condition": {"age": 1095} }
  ]
}
```

```bash
# Apply the lifecycle rules from a file to a bucket
gcloud storage buckets update gs://my-bucket --lifecycle-file=lifecycle.json
```

---

## Object Versioning = Undo Button ↩️
- Turn on **versioning** to keep **old copies** when a file is overwritten/deleted.
- *Analogy:* a **"version history"** like Google Docs — restore yesterday's version if
  you mess up today's.
- Combine with lifecycle rules to auto-delete really old versions (save money).

```bash
gcloud storage buckets update gs://my-bucket --versioning
```

---

## Saving Money with Classes + Lifecycle
- Keep only **active** data in **Standard**.
- Let **lifecycle rules** push aging data to colder, cheaper classes automatically.
- **Delete** what you no longer need.
- *Analogy:* don't pay **desk-side rent** for boxes you haven't opened in a year — let
  the robot move them to the **basement** or **toss** them.

---

## TL;DR (in plain English)
- **Storage classes = hot vs cold shelves**: Standard (hot) → Nearline → Coldline →
  Archive (cold, cheap to store, pricier to fetch, with minimum-stay fees).
- All classes have the **same speed**; they differ in **price**.
- **Lifecycle rules = a cleanup robot** that auto-moves or deletes aging files.
- **Versioning = an undo button** that keeps old copies.
- Combine them to **cut storage costs automatically**.
