# Images & Disks

Two things every VM needs: an **image** (the operating system) and a **disk** (where
data lives).

---

## Images = the Operating System

An **image** is a ready-made copy of an operating system (and sometimes software)
used to start a VM.

> **Analogy:** An image is like the **"factory settings" CD/USB** you use to install
> a fresh computer. Pick the image, and your VM boots up with that OS already set up.

### Types of images
- **Public images** — provided by Google/vendors: Debian, Ubuntu, CentOS, Windows Server, etc.
  - *Analogy:* buying a **brand-new laptop** with Windows/Linux pre-installed.
- **Custom images** — your *own* snapshot of a configured VM (your apps + settings baked in).
  - *Analogy:* making a **"master copy"** of your perfectly set-up computer so you can
    clone it again and again.
- **Image family** — a pointer that always gives the **latest** version
  (e.g., `debian-12` → newest Debian 12 image).
  - *Analogy:* "give me the **latest model**" instead of a specific year.

### Why custom images are useful
- Faster startup — software is already installed.
- Consistency — every VM is identical.
- *Analogy:* instead of setting up each new computer by hand, you **stamp out copies**
  from one perfect template.

---

## Disks = Storage for the VM

A **disk** is where the VM stores its OS, programs, and files.

> **Analogy:** The disk is the VM's **hard drive / cupboard** — where everything is kept.

### Boot disk vs data disk
- **Boot disk** — has the OS; the VM starts from it. (Created from the image.)
- **Data disk** — extra storage you attach for your files/databases.
  - *Analogy:* boot disk = the **main wardrobe** (with your clothes/OS);
    data disk = an **extra storage box** you add for more stuff.

### Types of Persistent Disks (network-attached, survive VM restarts)
| Type | Speed | Use it for | Analogy |
|---|---|---|---|
| **Standard (pd-standard, HDD)** | Slower, cheap | Backups, big sequential files | Slow but roomy cupboard |
| **Balanced (pd-balanced, SSD)** | Good all-rounder | Most workloads (default) | Reliable everyday shelf |
| **SSD (pd-ssd)** | Fast | Databases, high-traffic apps | Fast-access drawer |
| **Extreme (pd-extreme)** | Fastest | Heavy databases | Premium high-speed vault |

- **Persistent Disk** = stored on the network, **keeps your data** even if the VM stops.
  - *Analogy:* a **storage locker** separate from the car — the car can leave, the locker stays.

### Local SSD (temporary, super fast)
- Physically attached to the host, **very fast**, but **data is lost** when the VM stops.
- *Analogy:* a **scratchpad** on the desk — lightning fast, but wiped when you leave.

---

## Snapshots = Backups
A **snapshot** is a saved copy of a disk at a point in time.

> **Analogy:** Like taking a **photo** of your disk's contents. If something breaks,
> you can restore from the photo. Great for backups before risky changes.

```bash
# Take a snapshot (backup)
gcloud compute disks snapshot my-disk \
  --snapshot-names=my-disk-backup --zone=asia-south1-a
```

---

## Quick `gcloud` Reference
```bash
# List available public images
gcloud compute images list

# Create a VM from a specific image family (latest Ubuntu 22.04)
gcloud compute instances create my-vm \
  --image-family=ubuntu-2204-lts --image-project=ubuntu-os-cloud \
  --zone=asia-south1-a

# Add an extra data disk
gcloud compute disks create my-data --size=100GB --zone=asia-south1-a
gcloud compute instances attach-disk my-vm \
  --disk=my-data --zone=asia-south1-a

# Make a custom image from a disk (your "master copy")
gcloud compute images create my-app-image --source-disk=my-vm \
  --source-disk-zone=asia-south1-a
```

---

## TL;DR (in plain English)
- **Image = the OS install** (factory-settings USB). Public, custom, or "family" (latest).
- **Custom images** = a master template you can clone for fast, identical VMs.
- **Disk = the VM's hard drive.** Boot disk (OS) + optional data disks.
- **Persistent Disk** keeps data even when the VM stops; **Local SSD** is fast but temporary.
- **Snapshots = photo backups** of a disk.
