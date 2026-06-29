# Encryption & Retention

Two storage-security topics: **encryption** (keeping data unreadable to outsiders) and
**retention** (making sure data isn't deleted too early — or stays deleteable on time).

---

## 1. Encryption — locking your data 🔐

### Encryption is ON by default
GCP **automatically encrypts all data at rest** (on disk) and **in transit** (moving
over the network). You don't have to switch it on.

> **Analogy:** Every box in the warehouse is **locked in a safe by default**, and every
> delivery truck is **armored**. You get this for free.

The real question is just: **who controls the key?**

### The 3 key-management options
| Option | Who holds the key | Analogy |
|---|---|---|
| **Google-managed (default)** | Google manages keys for you | The **warehouse provides the lock & keeps the key** |
| **CMEK** (Customer-Managed Encryption Keys) | You manage keys in **Cloud KMS** | You **bring your own lock**, but store the key in Google's secure key cabinet |
| **CSEK** (Customer-Supplied) | You supply keys yourself, GCP never stores them | You bring your **own lock AND keep the only key** at home |

- **Default** — easiest; fine for most cases.
- **CMEK** — you want control: rotate, disable, or revoke keys (compliance). Uses **Cloud KMS**.
  - *Analogy:* you can **change the lock or throw away the key** anytime — instantly making
    the data unreadable.
- **CSEK** — maximum control, maximum responsibility (lose the key = lose the data).

> **Cloud KMS (Key Management Service)** = Google's secure service to create, rotate, and
> control encryption keys. **Secret Manager** (Day 2) is for secrets like passwords/API
> keys — different tool, related idea.

```bash
# Create a key in Cloud KMS, then use it as CMEK for a bucket
gcloud kms keyrings create my-ring --location=asia-south1
gcloud kms keys create my-key --location=asia-south1 \
  --keyring=my-ring --purpose=encryption
gcloud storage buckets update gs://my-bucket \
  --default-encryption-key=projects/PROJECT/locations/asia-south1/keyRings/my-ring/cryptoKeys/my-key
```

---

## 2. Retention — controlling when data can be deleted ⏳

Sometimes you must **keep** data for a legal period (compliance), or **prevent accidental
deletion**. Cloud Storage has tools for this.

### Retention policy (a minimum keep-time)
- Set a **minimum age** before any object can be deleted/overwritten.
- *Analogy:* a **"do not throw away before this date"** stamp on every box — even the
  owner can't bin it early.
- Useful for legal/compliance ("keep records 7 years").

### Object holds (freeze a specific object)
- **Temporary hold** / **event-based hold** — locks an individual object from deletion
  until the hold is removed.
- *Analogy:* putting a **single box in a locked evidence cage** during an investigation.

### Bucket Lock (make retention permanent)
- **Locks** a retention policy so it **can't be shortened or removed** — even by admins.
- *Analogy:* welding the **"keep until"** rule shut so **no one** can cheat it (regulators love this).

> ⚠️ Retention/lock vs **Lifecycle delete** (Day 5): lifecycle rules *delete* old data to
> save money; retention rules *prevent* deletion too early. They serve opposite goals —
> use carefully together.

```bash
# Require objects be kept at least 1 year (31536000 seconds)
gcloud storage buckets update gs://my-bucket --retention-period=31536000s

# Lock the retention policy permanently (irreversible!)
gcloud storage buckets update gs://my-bucket --lock-retention-period
```

---

## TL;DR (in plain English)
- **Encryption is automatic** (at rest + in transit) — the only choice is **who holds the key**:
  - **Google-managed** (default), **CMEK** (your keys in **Cloud KMS** — control/rotate/revoke),
    or **CSEK** (you keep the only key).
- **Cloud KMS** manages keys; **Secret Manager** stores passwords/secrets.
- **Retention policy** = a minimum "don't delete before" time; **holds** freeze single
  objects; **Bucket Lock** makes retention **permanent** for compliance.
- Retention **prevents** early deletion — the opposite of lifecycle **auto-delete**.
