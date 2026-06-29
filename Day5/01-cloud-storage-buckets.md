# Cloud Storage (Buckets)

## What is Cloud Storage?
Cloud Storage is GCP's service for storing **files (objects)** — images, videos,
backups, documents, logs, anything — in containers called **buckets**.

> **Analogy:** A **bucket** is like a **giant warehouse** (or a Google-Drive-style
> folder in the cloud) where you drop your files. You can store **unlimited** items,
> and grab any one whenever you need it.

This is called **object storage**: each file is an **object**, stored with its data +
some info (metadata) + a unique name.

---

## Buckets & Objects
| Term | Meaning | Analogy |
|---|---|---|
| **Bucket** | The container that holds your files | A **warehouse** / a labeled bin |
| **Object** | A single file stored in a bucket | A **box** sitting in the warehouse |
| **Object name** | The file's unique path/key | The box's **label/address** |

### Key facts
- A **bucket name is globally unique** across all of Google Cloud.
  - *Analogy:* like a **website domain** — no two can be the same.
- Buckets are **flat** (no real folders), but `/` in names *looks* like folders.
  - *Analogy:* boxes labeled `photos/2024/cat.jpg` *look* foldered but it's just the label.
- Objects are **immutable** — to "edit," you upload a new version (replace the box).

---

## What is it good for? ✅
- Website assets (images, CSS, JS), **static website hosting**.
- **Backups** and archives.
- Big data (analytics input/output), ML datasets.
- User uploads (profile pics, documents, videos).
- Sharing files via a link.

## What it's NOT for ❌
- Running an operating system or a database off it (that's what **disks** are for).
- *Analogy:* a warehouse is great for **storing boxes**, but you don't **live and work**
  inside a storage box — for that you need a proper room (a disk on a VM).

---

## Location Types (where the warehouse sits)
- **Region** — stored in one region (e.g., `asia-south1`). Cheapest, low latency locally.
- **Dual-region** — two specific regions (resilience + performance).
- **Multi-region** — a big area (`asia`, `us`, `eu`); highest availability.
  - *Analogy:* keeping **copies of your boxes in several cities** so they're always reachable.

---

## Access Control (who can open the warehouse)
- **IAM** — grant roles on the bucket (e.g., `roles/storage.objectViewer`).
- **Make public** — grant `allUsers` read access (for a public website/file).
  - ⚠️ *Analogy:* leaving the warehouse **door open to everyone** — only do this on
    purpose for truly public content.
- **Signed URLs** — a temporary, time-limited link to one object.
  - *Analogy:* a **visitor pass** that lets someone grab one box for one hour, then expires.

---

## Hosting a Static Website (mini lab idea)
You can serve a whole static site (HTML/CSS/JS) straight from a bucket.

```bash
# Create a bucket (must be globally unique)
gcloud storage buckets create gs://my-unique-site-bucket --location=asia-south1

# Upload files
gcloud storage cp index.html gs://my-unique-site-bucket/

# Make objects public (so the world can read them)
gcloud storage buckets add-iam-policy-binding gs://my-unique-site-bucket \
  --member=allUsers --role=roles/storage.objectViewer
```

---

## Quick `gcloud` Reference
```bash
# List buckets
gcloud storage buckets list

# Upload / download
gcloud storage cp myfile.txt gs://my-bucket/
gcloud storage cp gs://my-bucket/myfile.txt ./

# Copy a whole folder (recursive)
gcloud storage cp -r ./mydir gs://my-bucket/

# List objects in a bucket
gcloud storage ls gs://my-bucket/

# Delete an object / a bucket
gcloud storage rm gs://my-bucket/myfile.txt
gcloud storage rm -r gs://my-bucket
```

---

## TL;DR (in plain English)
- **Cloud Storage = a warehouse for files (objects)**, kept in **buckets**.
- **Bucket names are globally unique**; "folders" are just part of the file name.
- Great for **backups, media, uploads, static websites, big data** — not for OS/DBs.
- Choose a **region / dual / multi-region** location; control access with **IAM,
  public access, or signed URLs (visitor passes).**
