# Spot/Preemptible VMs & Pricing

## How VM Pricing Works (the basics)
You mostly pay for:
- **vCPU + RAM** while the VM is **running** (billed per second).
- **Disks** (even while the VM is stopped).
- **Network egress** (data leaving Google's network).
- Extras like external IPs, GPUs, etc.

> **Analogy:** Like a **taxi meter** — it runs while you're driving (VM running).
> Park the car (stop the VM) and the meter stops, but you still pay for the
> **parking spot** (the disk).

---

## Spot VMs (a.k.a. Preemptible) — cheap but interruptible

**Spot VMs** are spare Google capacity sold at a **huge discount (up to ~60–91% off)**.
The catch: Google can **take them back at any time** (with ~30 seconds warning).

> **Analogy:** Like flying **standby** or grabbing a **last-minute hotel deal**.
> Super cheap — but if a full-paying customer needs the seat/room, you get bumped.

### Good for ✅
- Batch jobs, data processing, rendering, CI/CD.
- **Anything that can stop and restart** without harm.
- *Analogy:* tasks you can **pause and resume** anytime.

### Bad for ❌
- Databases, websites, anything that must **stay up 24/7**.
- *Analogy:* you wouldn't book **standby flights** for your own **wedding day**.

### Spot vs Preemptible (quick note)
- **Preemptible** = the older version (max 24-hour life).
- **Spot** = the newer version (no 24-hour limit, otherwise similar). Prefer **Spot**.

---

## Discount Options (ways to pay less)

| Discount | What it is | Analogy |
|---|---|---|
| **Pay-as-you-go (on-demand)** | Standard per-second price | Full-price taxi ride |
| **Sustained Use Discounts** | **Automatic** discount for running a VM most of the month | Loyalty discount for being a regular |
| **Committed Use Discounts (CUDs)** | Commit to 1 or 3 years → big savings | A **gym membership** — cheaper if you commit long-term |
| **Spot VMs** | Spare capacity, can be reclaimed | Standby / last-minute deal |

### When to use which
- **Steady, always-on workload** → **Committed Use** (commit 1–3 yrs, save most).
- **Runs a lot but not committed** → **Sustained Use** (automatic, no action needed).
- **Flexible, interruptible work** → **Spot VMs** (cheapest, but can be bumped).
- **Short experiments** → **on-demand**, then delete.

---

## Money-Saving Habits
- ✅ **Stop or delete** VMs when not in use (delete = no disk charge either).
- ✅ **Right-size** — don't rent a truck to carry a bag (use the smallest type that works).
- ✅ Use **Spot VMs** for interruptible jobs.
- ✅ Use **CUDs** for predictable, long-running workloads.
- ✅ Watch **network egress** (downloading lots of data out costs money).
- ✅ Set a **budget + alert** (Day 1!).

---

## Quick `gcloud` Reference
```bash
# Create a Spot VM (cheap, interruptible)
gcloud compute instances create batch-vm \
  --zone=asia-south1-a \
  --machine-type=e2-medium \
  --provisioning-model=SPOT \
  --instance-termination-action=DELETE

# Compare: a normal (on-demand) VM
gcloud compute instances create web-vm \
  --zone=asia-south1-a --machine-type=e2-medium
```

---

## TL;DR (in plain English)
- You pay for **CPU+RAM while running**, **disks always**, and **data leaving GCP**.
- **Spot VMs** = up to ~91% cheaper but can be **taken back anytime** → use for
  pause/resume-able jobs, never for 24/7 services.
- Discounts: **Sustained** (automatic), **Committed** (commit 1–3 yrs, biggest save), **Spot** (cheapest/riskiest).
- Save money: **stop/delete idle VMs, right-size, use the right discount, set budgets.**
