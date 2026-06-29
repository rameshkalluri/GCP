# Compute Engine Basics

## What is Compute Engine?
Compute Engine is GCP's service for creating **Virtual Machines (VMs)** — computers
you rent in Google's data centers.

> **Analogy:** Instead of **buying a PC** (expensive, you maintain it), you **rent
> one by the second** from Google. Need a bigger one? Swap it. Done for the day?
> Turn it off and stop paying. Like **renting a car** vs. owning one.

A VM is also called an **instance**.

---

## What makes up a VM?
When you create a VM, you choose:

| Choice | What it means | Analogy |
|---|---|---|
| **Machine type** | How many CPUs + how much RAM | Engine size of the rental car |
| **Image** | The operating system (Linux/Windows) | Which "model" the car is |
| **Disk** | Storage attached to the VM | The car's trunk space |
| **Region/Zone** | Where it physically lives | Which city you rent in |
| **Network** | How it connects (VPC, firewall) | The roads it can drive on |

---

## Machine Types (sizing your VM)
A **machine type** = a preset combination of **vCPUs** (processing power) and
**memory (RAM)**.

> **Analogy:** Like choosing a vehicle: a **scooter** (tiny), a **sedan** (balanced),
> a **truck** (heavy lifting), or a **race car** (super fast). Pick what the job needs.

### Families (simple view)
- **General-purpose (E2, N2, N4...)** — balanced, everyday workloads (web apps, small DBs).
  - *Analogy:* the **family sedan** — good for most trips.
- **Compute-optimized (C2, C3...)** — lots of CPU power (gaming, heavy compute).
  - *Analogy:* the **race car**.
- **Memory-optimized (M1, M2...)** — huge RAM (big in-memory databases).
  - *Analogy:* the **moving truck** with a giant cargo hold.
- **Accelerator-optimized (A2, G2...)** — GPUs for AI/ML.
  - *Analogy:* a **specialized vehicle** for a special job.

### Custom machine types
- Can't find the perfect fit? Build your own (e.g., 4 vCPU + 10 GB RAM).
- *Analogy:* a **made-to-order car** with exactly the engine and seats you want.

---

## The VM Lifecycle
```
CREATE → RUNNING → (STOP) → STOPPED → (START) → RUNNING → DELETE
```
- **Running** — switched on, you pay for CPU/RAM.
- **Stopped** — switched off; you stop paying for CPU/RAM but **still pay for the disk**
  (your data is kept).
  - *Analogy:* a parked rental car — you're not burning fuel, but you still pay for the
    parking spot (the disk).
- **Deleted** — gone (and its data, unless the disk is kept).

---

## Connecting to a VM
- **Linux VMs** → **SSH** (secure shell). One-click "SSH" button in the Console, or:
  ```bash
  gcloud compute ssh my-vm --zone=asia-south1-a
  ```
- **Windows VMs** → **RDP** (Remote Desktop).

---

## Quick `gcloud` Reference
```bash
# Create a small Linux VM
gcloud compute instances create my-vm \
  --zone=asia-south1-a \
  --machine-type=e2-medium \
  --image-family=debian-12 --image-project=debian-cloud

# List VMs
gcloud compute instances list

# Stop / start (save money when idle)
gcloud compute instances stop my-vm --zone=asia-south1-a
gcloud compute instances start my-vm --zone=asia-south1-a

# SSH into it
gcloud compute ssh my-vm --zone=asia-south1-a

# Delete it (stop paying completely)
gcloud compute instances delete my-vm --zone=asia-south1-a
```

---

## TL;DR (in plain English)
- **Compute Engine = renting computers (VMs)** in Google's data center.
- **Machine type** = how big (CPU + RAM) → pick like choosing a vehicle for the job.
- A VM also needs an **image (OS)**, a **disk (storage)**, and a **zone**.
- **Stopped** VMs still cost for the disk; **delete** to stop paying entirely.
- Connect via **SSH** (Linux) or **RDP** (Windows).
