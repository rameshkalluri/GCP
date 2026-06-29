# Disks & Filestore

Buckets are great for **files you grab over the internet**. But a running **VM** needs
a real **hard drive** to boot and work from — and sometimes **many VMs** need to share
the **same folder**. That's where **disks** and **Filestore** come in.

---

## Quick mental model: 3 ways to store data

| Type | What it is | Analogy |
|---|---|---|
| **Object storage** (Cloud Storage) | Files you fetch by name over the internet | A **warehouse** of boxes |
| **Block storage** (Persistent Disk) | A raw "hard drive" attached to one VM | The **hard drive inside your computer** |
| **File storage** (Filestore) | A shared network folder many VMs mount | A **shared office drive** everyone maps |

---

## Persistent Disks (block storage for a VM)

A **Persistent Disk (PD)** is a virtual **hard drive** attached to a VM over the network.

> **Analogy:** It's the **hard drive of your rented computer** — where the OS, programs,
> and files live. But unlike a physical drive bolted inside, it's actually a **storage
> locker on the network**, so it can survive even if the VM is deleted.

### Key facts (recap from Day 3)
- **Survives VM stop/restart** — your data stays.
- Types: **Standard (HDD)**, **Balanced (SSD)**, **SSD**, **Extreme** — slower/cheaper to
  faster/pricier.
- Can be **resized** while in use (grow bigger).
- **Snapshots** = photo backups of a disk.

### Local SSD (the exception)
- Physically attached, **super fast**, but **temporary** — data is **lost** when the VM
  stops.
- *Analogy:* a **scratchpad on the desk** — lightning fast, wiped when you leave.

> Use a PD when **one VM** needs its own drive.

---

## Filestore (shared file storage)

**Filestore** is a fully-managed **shared network file system (NFS)** that **many VMs
can mount and use at the same time**.

> **Analogy:** Like a **shared office drive (the "S: drive")** that every employee's
> computer maps to. Everyone reads/writes the **same folder**, so files are shared
> instantly across the team.

### When to use Filestore
- Multiple VMs need to **read/write the same files** simultaneously.
- Apps that expect a **traditional file system** path (e.g., legacy apps, content
  management, shared media, render farms).
- *Analogy:* a film studio where **many editors** open the **same project folder**.

### Persistent Disk vs Filestore
| | **Persistent Disk** | **Filestore** |
|---|---|---|
| Shared by | Usually **one VM** | **Many VMs** at once |
| Looks like | A raw hard drive | A shared folder (NFS) |
| Analogy | Your **personal hard drive** | The **shared office drive** |
| Use for | A single VM's OS/data | Teams/apps sharing files |

---

## Putting the 3 together
```
Cloud Storage (buckets) → backups, media, web assets, big data  (warehouse)
Persistent Disk         → a single VM's hard drive               (your PC drive)
Filestore               → a shared folder for many VMs           (office S: drive)
```

---

## Quick `gcloud` Reference
```bash
# Persistent Disk: create + attach to a VM
gcloud compute disks create data-disk --size=100GB --type=pd-balanced \
  --zone=asia-south1-a
gcloud compute instances attach-disk my-vm --disk=data-disk --zone=asia-south1-a

# Snapshot (backup) a disk
gcloud compute disks snapshot data-disk \
  --snapshot-names=data-backup --zone=asia-south1-a

# Filestore: create a shared file share
gcloud filestore instances create my-share \
  --zone=asia-south1-a --tier=BASIC_HDD \
  --file-share=name=share1,capacity=1TB \
  --network=name=my-vpc
```

---

## TL;DR (in plain English)
- Three storage shapes: **object (buckets) = warehouse**, **block (Persistent Disk) =
  a VM's hard drive**, **file (Filestore) = a shared office drive**.
- **Persistent Disk** = one VM's drive; survives restarts; pick HDD→SSD by speed need.
- **Local SSD** = super fast but **temporary** (wiped on stop).
- **Filestore** = a **shared folder many VMs mount** at once (NFS).
