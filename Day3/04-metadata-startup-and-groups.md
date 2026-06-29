# Metadata, Startup Scripts & Instance Groups

This is where VMs become **automatic and self-scaling** instead of hand-managed.

---

## 1. Instance Metadata

Every VM has a **metadata** store — a set of **key/value notes** about the VM that
the VM (and you) can read.

> **Analogy:** Like a **sticky note / info card** attached to the rental car glovebox:
> "Owner: Alice, Purpose: web server, Config: X." The car can read its own note.

- Includes built-in info (VM name, zone, IP) and **custom** values you add.
- The VM reads it from a special internal address (`metadata.google.internal`).
- Great for passing **config** to a VM without hardcoding it.

```bash
# Add custom metadata
gcloud compute instances add-metadata my-vm \
  --metadata=environment=prod,team=payments --zone=asia-south1-a
```

---

## 2. Startup Scripts (auto-setup on boot)

A **startup script** runs **automatically every time the VM boots**, so the machine
sets *itself* up — no manual SSH-and-install needed.

> **Analogy:** Like a **"to-do checklist" taped to the door** that the VM follows the
> moment it wakes up: "1) update system, 2) install web server, 3) start it."

### Example: auto-install a web server
```bash
gcloud compute instances create web-vm \
  --zone=asia-south1-a --machine-type=e2-small \
  --image-family=debian-12 --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
apt-get update
apt-get install -y nginx
echo "Hello from $(hostname)" > /var/www/html/index.html'
```

Why it's powerful:
- New VMs are **ready-to-go** automatically.
- Essential for **autoscaling** (new VMs must configure themselves with no human).
- *Analogy:* every new employee gets the **same onboarding checklist** and is
  productive immediately.

---

## 3. Instance Templates (a blueprint)

An **instance template** saves a VM recipe (machine type, image, disk, startup script,
network) so you can stamp out identical VMs.

> **Analogy:** A **cookie cutter** (or a saved order: "1 medium VM, Debian, with nginx").
> Press it to get the exact same VM every time.

```bash
gcloud compute instance-templates create web-template \
  --machine-type=e2-small \
  --image-family=debian-12 --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
apt-get update && apt-get install -y nginx'
```

---

## 4. Managed Instance Groups (MIGs) — auto-scaling & self-healing

A **Managed Instance Group** runs **many identical VMs** (made from a template) and
manages them as **one unit**.

> **Analogy:** Instead of hiring one cashier, you run a **team of identical cashiers**.
> A manager (the MIG) automatically:
> - **adds more cashiers when lines get long** (autoscaling up),
> - **sends some home when it's quiet** (autoscaling down),
> - **replaces any cashier who gets sick** (self-healing).

### Superpowers of a MIG
- **Autoscaling** — add/remove VMs based on CPU, load-balancer traffic, etc.
  - *Analogy:* more staff at rush hour, fewer at midnight.
- **Autohealing** — if a VM fails a health check, it's **recreated** automatically.
  - *Analogy:* a broken cashier is swapped for a fresh one — no manager needed.
- **Rolling updates** — update VMs gradually with no downtime.
  - *Analogy:* retrain staff **one at a time** so the shop never closes.
- **Regional spread** — place VMs across **multiple zones** for resilience (see Day 4 HA).

### Zonal vs Regional MIG
- **Zonal MIG** — all VMs in one zone (simpler, less resilient).
- **Regional MIG** — VMs spread across zones (survives a zone outage). **Preferred for prod.**

```bash
# Create an autoscaling, multi-zone group of web servers
gcloud compute instance-groups managed create web-mig \
  --template=web-template --size=2 --region=asia-south1

# Turn on autoscaling (2–10 VMs, target 60% CPU)
gcloud compute instance-groups managed set-autoscaling web-mig \
  --region=asia-south1 \
  --min-num-replicas=2 --max-num-replicas=10 \
  --target-cpu-utilization=0.6
```

> Tip: A MIG usually sits **behind a Load Balancer** (Day 4), which spreads traffic
> across all the VMs.

---

## 5. Unmanaged Instance Groups (UMIGs)

An **Unmanaged Instance Group (UMIG)** is just a **bag of existing VMs grouped together**
that **you manage by hand**. There's **no template, no autoscaling, no autohealing**.

> **Analogy:** If a MIG is a **self-managing team of identical cashiers** (a manager
> auto-hires, replaces, and retrains them), a UMIG is a **fixed group of different,
> hand-picked workers** you supervise yourself. Nobody gets auto-hired or auto-replaced —
> if one quits, **you** sort it out. You just **clip them together** so a load balancer
> can send work to the whole group.

### Key facts
- VMs can be **different** (different machine types, images, configs).
  - *Analogy:* a mixed team — a cashier, a bagger, a greeter — not clones.
- **No autoscaling, no autohealing, no rolling updates** — all manual.
- Mainly used to **group existing VMs behind a load balancer**.
- *Analogy:* a **name list taped to the wall** — "these specific people are on this team."

### When to use a UMIG
- You have **pre-existing, individually-configured VMs** you want behind one load balancer.
- You need VMs that are **not identical** in the same group.
- Quick/legacy setups where you don't need scaling or healing.
- 👉 For most modern, scalable apps, **prefer a MIG**.

---

## MIG vs UMIG (side by side)

| Feature | **MIG (Managed)** | **UMIG (Unmanaged)** |
|---|---|---|
| VMs are... | **Identical** (from a template) | **Different**, hand-picked |
| Created from a **template** | ✅ Yes | ❌ No |
| **Autoscaling** | ✅ Yes | ❌ No |
| **Autohealing** | ✅ Yes | ❌ No |
| **Rolling updates** | ✅ Yes | ❌ No |
| Multi-zone (regional) | ✅ Yes | ❌ No (zonal only) |
| Who manages the VMs | **GCP (the manager)** | **You, manually** |
| Analogy | Self-managing team of **clones** | Hand-picked **mixed** team you supervise |
| Best for | Scalable, self-healing production apps | Grouping existing/odd VMs behind an LB |

```bash
# Create an unmanaged instance group and add existing VMs to it
gcloud compute instance-groups unmanaged create my-umig --zone=asia-south1-a
gcloud compute instance-groups unmanaged add-instances my-umig \
  --instances=vm-1,vm-2 --zone=asia-south1-a
```

> Rule of thumb: **MIG = automatic & identical; UMIG = manual & mixed.**

---

## How it all fits together
```
Startup Script  →  baked into  →  Instance Template  →  used by  →  Managed Instance Group
                                                                   ↑   ↓
                                                        autoscaling / autohealing / rolling updates
```
*Analogy:* a **checklist** → goes into a **recipe** → used by a **manager** who runs a
**self-managing team** that grows, shrinks, and heals on its own.

---

## TL;DR (in plain English)
- **Metadata** = a sticky note of config the VM can read about itself.
- **Startup script** = a to-do checklist the VM runs automatically when it boots.
- **Instance template** = a cookie-cutter recipe for identical VMs.
- **Managed Instance Group (MIG)** = a self-managing team of VMs that **autoscales**,
  **self-heals**, and does **rolling updates** — use a **regional** MIG for production.
- **Unmanaged Instance Group (UMIG)** = a manual bag of **different, hand-picked VMs**
  grouped behind a load balancer — **no autoscaling/healing**. **MIG = automatic & identical;
  UMIG = manual & mixed.**
