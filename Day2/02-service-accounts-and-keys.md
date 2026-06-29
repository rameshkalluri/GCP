# Service Accounts & Keys

## What is a Service Account?
A **service account (SA)** is an identity for a **robot, app, or machine** — not a human.

> **Analogy:** Humans log in with their own name badge. But what about the
> **cleaning robot**, the **delivery drone**, or the **automatic door**? They each
> need their **own ID badge** too. A service account is that **robot's ID badge**.

When your app (running on a VM, Cloud Run, etc.) needs to read a file from Cloud
Storage, it doesn't use *your* login — it uses **its own service account**.

---

## Two Sides of a Service Account
A service account is special because it is **both an identity AND a resource**:

1. **What the SA can do** → you give the SA **roles** (key cards).
   - e.g. SA `report-bot` gets `roles/storage.objectViewer` → it can read files.
2. **Who can use the SA** → you let certain people **"act as"** the SA.
   - *Analogy:* who is allowed to **pick up and use the robot's badge**.

---

## Service Account Keys (handle with care! 🔑)

A **key** is like a **physical copy of the robot's badge** that you can carry around
(a JSON file) to prove "I am this service account" from outside GCP.

### The problem with keys
- *Analogy:* It's like **photocopying your house key** and leaving copies around.
  If someone finds one, they can get in — and copies **never expire** on their own.
- Leaked keys are one of the **most common cloud security incidents**.

### Better alternatives (avoid downloading keys when you can)
- **Attached service accounts** — a VM/Cloud Run service simply *runs as* an SA,
  no key file needed. *Analogy:* the badge is **built into the robot**, not carried loose.
- **Workload Identity Federation** — let external/other-cloud workloads borrow access
  **without a key file**.

### If you must use a key
- Store it secretly (e.g., **Secret Manager**), never in code or Git.
- **Rotate** (replace) it regularly and **delete** unused ones.

---

## Best Practices (simple rules)
- ✅ **One service account per app/job** (don't share one robot badge for everything).
- ✅ Give it the **least access** it needs.
- ✅ Prefer **attached SAs / Workload Identity** over downloaded keys.
- ✅ **Rotate and delete** old keys.
- ❌ Never commit a key file to Git or paste it in code.
- ❌ Don't use the **default** service account with broad access for real workloads.

---

## Quick `gcloud` Reference

```bash
# Create a service account (give the robot a badge)
gcloud iam service-accounts create report-bot \
  --display-name="Report Generator Bot"

# Let the SA read Cloud Storage (give it a key card)
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:report-bot@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/storage.objectViewer"

# List service accounts
gcloud iam service-accounts list

# (Only if truly needed) create a key file
gcloud iam service-accounts keys create key.json \
  --iam-account=report-bot@PROJECT_ID.iam.gserviceaccount.com

# List keys for an SA (to audit/rotate)
gcloud iam service-accounts keys list \
  --iam-account=report-bot@PROJECT_ID.iam.gserviceaccount.com
```

---

## TL;DR (in plain English)
- A **service account = an ID badge for a robot/app**, not a person.
- It has **two sides**: what it can do (roles) and who can act as it.
- **Keys = loose copies of the badge** → risky; avoid downloading them.
- Prefer **attached SAs / Workload Identity**; if you use keys, **secure, rotate, delete**.
- One SA per app, least privilege, never put keys in Git.
