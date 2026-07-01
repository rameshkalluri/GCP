# Firestore & NoSQL

## What is Firestore?
**Firestore** is a **fully managed, serverless NoSQL document database** — great for
web/mobile apps that need flexible data and **real-time** updates.

> **Analogy:** If SQL is a strict **ledger with fixed columns**, Firestore is a box of
> **sticky notes (documents)** grouped into **folders (collections)**. Each note can
> hold different fields, and it **auto-scales** without you managing servers.

---

## The data model
```
Collection (e.g., "users")
   └── Document (e.g., "user123")   ← a record, like a JSON object
          ├── field: name = "Alice"
          ├── field: age = 30
          └── Subcollection (e.g., "orders")
                 └── Document ("order1") { total: 500 }
```
- **Collection** = a folder of documents.
- **Document** = a record (key/value fields, like JSON).
- Documents can hold **subcollections** (nesting).

---

## Why developers love it
- **Serverless & auto-scaling** — no capacity planning.
- **Real-time listeners** — clients get **live updates** when data changes.
  - *Analogy:* the sticky note **updates on everyone's board instantly**.
- **Offline support** — mobile SDKs cache data and sync later.
- **Strong consistency** for document reads.
- **Security Rules** — fine-grained access control for client apps.

---

## Firestore vs Datastore
- **Datastore** was the older version; **Firestore in Native mode** is the current,
  recommended choice (Datastore mode exists for backward compatibility).

---

## When to use Firestore ✅
- Web/mobile app backends (user profiles, chat, to-do lists, game state).
- Flexible/changing data shapes.
- Need **real-time sync** and **offline** on devices.

## When NOT to use it ❌
- Complex multi-table **joins / heavy relational** queries → **Cloud SQL/Spanner**.
- **Analytics** over huge datasets → **BigQuery**.
- **Petabyte-scale, ultra-high write** time-series/IoT → **Bigtable**.

---

## Quick example (concept)
```bash
# Firestore is typically used via client SDKs (JS, Python, etc.), e.g. Python:
# from google.cloud import firestore
# db = firestore.Client()
# db.collection("users").document("user123").set({"name": "Alice", "age": 30})
# doc = db.collection("users").document("user123").get()
```

```bash
# Create a Firestore database (Native mode)
gcloud firestore databases create --location=asia-south1
```

---

## TL;DR (in plain English)
- **Firestore = serverless NoSQL document database** — sticky notes (documents) in
  folders (collections).
- **Auto-scales**, supports **real-time updates** and **offline** sync — perfect for
  web/mobile apps.
- Use it for **flexible app data**, not for heavy joins (**SQL/Spanner**), analytics
  (**BigQuery**), or petabyte time-series (**Bigtable**).
