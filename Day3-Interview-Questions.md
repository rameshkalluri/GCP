# GCP Day 3 — Interview Questions & Answers

A curated set of interview questions with answers covering **Day 3 — Compute Engine
(Virtual Machines)**: VM basics & machine types, images & disks, Spot VMs & pricing,
metadata/startup scripts/instance groups (MIG vs UMIG), and VM access & special options.

---

## Compute Engine Basics & Machine Types

**Q1. What is Compute Engine and what is an "instance"?**
Compute Engine is GCP's **IaaS** service for creating **Virtual Machines (VMs)** — computers you rent in Google's data centers, billed per second. A VM is also called an **instance**.

**Q2. What do you choose when creating a VM?**
**Machine type** (vCPUs + RAM), **image** (OS), **disk** (storage), **region/zone** (location), and **network** (VPC + firewall).

**Q3. What are the machine type families and when do you use each?**
- **General-purpose (E2, N2, N4)** — balanced everyday workloads (web apps, small DBs).
- **Compute-optimized (C2, C3)** — high CPU (gaming, heavy compute).
- **Memory-optimized (M1, M2)** — huge RAM (large in-memory databases).
- **Accelerator-optimized (A2, G2)** — GPUs for AI/ML.
- **Custom machine types** — build your own vCPU/RAM combo when no preset fits.

**Q4. Describe the VM lifecycle and the cost implications.**
`CREATE → RUNNING → STOP → STOPPED → START → RUNNING → DELETE`.
- **Running** — you pay for CPU/RAM.
- **Stopped** — you stop paying for CPU/RAM but **still pay for the disk** (data is kept).
- **Deleted** — gone entirely (data lost unless the disk is retained).

**Q5. How do you connect to a VM?**
**SSH** for Linux (`gcloud compute ssh my-vm --zone=...` or the Console SSH button); **RDP** for Windows.

---

## Images & Disks

**Q6. What is an image? What are the types?**
An image is a ready-made copy of an OS (and optionally software) used to boot a VM.
- **Public images** — provided by Google/vendors (Debian, Ubuntu, Windows Server).
- **Custom images** — your own snapshot of a configured VM (apps + settings baked in).
- **Image family** — a pointer that always resolves to the **latest** version (e.g., `debian-12`).

**Q7. Why use custom images?**
Faster startup (software pre-installed) and consistency (every VM is identical) — like stamping out copies from one perfect template. Essential for fast, repeatable, autoscaling deployments.

**Q8. Boot disk vs data disk?**
**Boot disk** holds the OS and the VM starts from it (created from the image). **Data disk** is extra storage you attach for files/databases.

**Q9. Compare the Persistent Disk types.**
- **Standard (pd-standard, HDD)** — slower, cheap; backups, big sequential files.
- **Balanced (pd-balanced, SSD)** — good all-rounder; the default for most workloads.
- **SSD (pd-ssd)** — fast; databases, high-traffic apps.
- **Extreme (pd-extreme)** — fastest; heavy databases.

**Q10. Persistent Disk vs Local SSD?**
**Persistent Disk** is network-attached and **keeps data even when the VM stops** (like a separate storage locker). **Local SSD** is physically attached, **very fast, but data is lost when the VM stops** (like a scratchpad).

**Q11. What is a snapshot?**
A point-in-time backup copy of a disk — like taking a photo of the disk's contents so you can restore it later (great before risky changes).

---

## Spot VMs & Pricing

**Q12. What do you pay for with a VM?**
**vCPU + RAM** while running (per-second), **disks** (even while stopped), **network egress** (data leaving Google's network), and extras (external IPs, GPUs, etc.). Analogy: a taxi meter that runs while driving, but you still pay for the parking spot (disk) when stopped.

**Q13. What are Spot VMs and their trade-off?**
Spare Google capacity sold at **up to ~60–91% off**, but Google can **reclaim them at any time** (~30 seconds warning). Good for batch jobs, data processing, rendering, CI/CD — anything that can stop/restart. Bad for databases, websites, or anything that must stay up 24/7.

**Q14. Spot vs Preemptible — what's the difference?**
**Preemptible** is the older version with a **max 24-hour lifetime**. **Spot** is the newer version with **no 24-hour limit** (otherwise similar). Prefer **Spot**.

**Q15. Explain the discount options and when to use each.**
- **On-demand (pay-as-you-go)** — standard price; short experiments.
- **Sustained Use Discounts** — **automatic** discount for running a VM most of the month.
- **Committed Use Discounts (CUDs)** — commit 1–3 years for the **biggest savings**; best for steady, always-on workloads.
- **Spot VMs** — cheapest but interruptible; best for flexible/interruptible work.

---

## Metadata, Startup Scripts & Instance Groups

**Q16. What is instance metadata?**
A per-VM **key/value store** the VM (and you) can read — built-in info (name, zone, IP) plus custom values. The VM reads it from `metadata.google.internal`. Useful for passing config without hardcoding.

**Q17. What is a startup script and why is it important for autoscaling?**
A script that runs **automatically every time the VM boots**, so the machine sets itself up (e.g., update, install nginx, start it). It's essential for **autoscaling** because newly created VMs must configure themselves with no human intervention.

**Q18. What is an instance template?**
A saved VM "recipe" (machine type, image, disk, startup script, network) used to stamp out identical VMs — like a cookie cutter. It's the blueprint a MIG uses.

**Q19. What is a Managed Instance Group (MIG) and what are its superpowers?**
A MIG runs many identical VMs (from a template) managed as one unit. Superpowers:
- **Autoscaling** — add/remove VMs based on CPU, LB traffic, etc.
- **Autohealing** — recreate VMs that fail a health check.
- **Rolling updates** — update VMs gradually with no downtime.
- **Regional spread** — place VMs across multiple zones for resilience.

**Q20. Zonal vs Regional MIG?**
**Zonal MIG** keeps all VMs in one zone (simpler, less resilient). **Regional MIG** spreads VMs across zones to survive a zone outage — **preferred for production**.

**Q21. What is an Unmanaged Instance Group (UMIG) and how does it differ from a MIG?**
A UMIG is a **manually managed bag of existing, possibly different VMs** grouped (usually behind a load balancer). It has **no template, no autoscaling, no autohealing, no rolling updates, zonal only**. Rule of thumb: **MIG = automatic & identical; UMIG = manual & mixed.** Prefer MIG for modern scalable apps.

**Q22. How do startup script, template, and MIG fit together?**
`Startup Script → baked into → Instance Template → used by → Managed Instance Group` (which then autoscales, autoheals, and does rolling updates). A MIG typically sits behind a load balancer.

---

## VM Access & Special Options

**Q23. OS Login vs metadata SSH keys?**
**Metadata SSH keys** = manually add public keys to VM/project metadata (messy at scale). **OS Login** (recommended) ties SSH access to a user's **Google identity + IAM roles**, so adding/removing access in IAM updates everywhere automatically. Revoke one IAM role and the person is locked out of every VM. Roles: `roles/compute.osLogin` (normal) and `roles/compute.osAdminLogin` (sudo).

**Q24. What is a Shielded VM? And a Confidential VM?**
**Shielded VM** adds secure boot, vTPM, and integrity monitoring to ensure the VM **boots clean and untampered** (a tamper-proof seal). **Confidential VM** goes further — it **encrypts data even while in use (in memory)**.

**Q25. What are sole-tenant nodes?**
VMs that run on a **physical server dedicated to you** (not shared with other customers) — used for compliance/licensing needs. Like renting a whole floor to yourself.

**Q26. GPUs vs TPUs?**
**GPUs** add graphics/AI processing power (ML training, rendering). **TPUs** are Google's custom chips built specifically for machine learning. **Reservations** let you book capacity in advance so VMs are guaranteed available.

---

## Scenario / Cross-Topic Questions

**Q27. You need to run a nightly batch job that's fault-tolerant and cheap. What do you use?**
**Spot VMs** (up to ~91% cheaper) since the job can stop/restart, ideally in a MIG so failed/preempted instances are recreated automatically. Set `--instance-termination-action=DELETE`.

**Q28. Your web app needs to handle traffic spikes and survive a zone outage. Design it.**
Build an **instance template** with a **startup script**, run a **regional MIG** with **autoscaling** (min/max replicas, target CPU) spread across multiple zones, enable **autohealing** with health checks, and put it behind a load balancer (Day 4).

**Q29. You stopped your VM to save money but the bill isn't zero. Why?**
A **stopped VM still incurs disk charges** (and reserved static IPs). To stop paying entirely, **delete** the VM (and its disks) or release unused resources.

**Q30. A teammate left the company. How do you instantly revoke their SSH access to all VMs?**
If **OS Login** is enabled, just remove their IAM role (`roles/compute.osLogin`/`osAdminLogin`) — they lose access to every VM at once. With metadata SSH keys you'd have to remove their key from each VM/project.

**Q31. You need every new VM to have nginx installed and running with zero manual steps. How?**
Use a **startup script** (passed via `--metadata=startup-script=...`), ideally baked into an **instance template** or a **custom image**, so each VM self-configures on boot.

---

## Quick `gcloud` Commands to Remember

```bash
# Create / list / stop / start / delete a VM
gcloud compute instances create my-vm \
  --zone=asia-south1-a --machine-type=e2-medium \
  --image-family=debian-12 --image-project=debian-cloud
gcloud compute instances list
gcloud compute instances stop my-vm --zone=asia-south1-a
gcloud compute instances start my-vm --zone=asia-south1-a
gcloud compute instances delete my-vm --zone=asia-south1-a

# SSH in
gcloud compute ssh my-vm --zone=asia-south1-a

# Images, disks, snapshots
gcloud compute images list
gcloud compute disks create my-data --size=100GB --zone=asia-south1-a
gcloud compute disks snapshot my-disk --snapshot-names=my-disk-backup --zone=asia-south1-a

# Spot VM
gcloud compute instances create batch-vm \
  --zone=asia-south1-a --machine-type=e2-medium \
  --provisioning-model=SPOT --instance-termination-action=DELETE

# Instance template + regional MIG + autoscaling
gcloud compute instance-templates create web-template \
  --machine-type=e2-small \
  --image-family=debian-12 --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
apt-get update && apt-get install -y nginx'
gcloud compute instance-groups managed create web-mig \
  --template=web-template --size=2 --region=asia-south1
gcloud compute instance-groups managed set-autoscaling web-mig \
  --region=asia-south1 --min-num-replicas=2 --max-num-replicas=10 \
  --target-cpu-utilization=0.6

# OS Login
gcloud compute project-info add-metadata --metadata enable-oslogin=TRUE
```
