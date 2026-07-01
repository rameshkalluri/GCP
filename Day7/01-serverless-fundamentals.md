# Serverless Fundamentals

## What does "Serverless" mean?
**Serverless** doesn't mean *no servers* — it means **you don't manage them**. Google
provisions, scales, patches, and heals the servers; you just deploy **code or containers**.

> **Analogy:** Like **electricity from a socket** — you don't run a power plant, you
> just plug in and pay for what you use. Or a **taxi vs owning a car**: no parking,
> insurance, or maintenance; you pay per ride.

---

## The 3 hallmarks of serverless

### 1. No server management
- No OS patching, no capacity planning, no VM babysitting.
- *Analogy:* you cook the meal; the **restaurant** handles the kitchen.

### 2. Scale to zero (and up automatically)
- Idle → **zero instances → (near) zero cost**.
- Traffic spike → **auto-scales up** in seconds; drops back down after.
- *Analogy:* the lights **turn off when the room is empty** and on when someone enters.

### 3. Pay-per-use
- Billed by **actual usage** (requests, compute time, memory), often to the **millisecond**.
- *Analogy:* a **pay-as-you-go phone** — no bill when you don't call.

---

## Cold Starts (the main trade-off) 🥶
When a service has scaled to zero, the **first request** must **spin up a new instance**,
adding a small startup delay called a **cold start**.

> **Analogy:** The shop was **closed** (zero cost). The first customer has to **wait for
> the shutters to open and lights to warm up**. After that, service is instant (warm).

- Later requests hit a **warm** instance = fast.
- Mitigations: **minimum instances** (keep some warm), lighter runtimes, smaller images.

---

## GCP's serverless compute options (preview)
| Service | You deploy | Best for |
|---|---|---|
| **Cloud Functions** | A single **function** | Event-driven glue, small APIs |
| **Cloud Run** | A **container** | Web apps/APIs, any language, full control |
| **App Engine** | An **app** (code) | Traditional web apps on a managed platform |

> Also "serverless" in spirit: **BigQuery, Firestore, Pub/Sub, Cloud Storage** — no
> servers to manage.

---

## Serverless vs VMs vs Containers (mindset)
- **VM (Compute Engine)** — you manage the whole machine (most control, most ops).
- **Container (GKE)** — you manage the cluster/orchestration.
- **Serverless** — you manage **only your code**; Google does the rest (least ops).

---

## TL;DR (in plain English)
- **Serverless = you manage only your code**; Google runs/scales/patches the servers.
- Hallmarks: **no server management**, **scale to zero**, **pay-per-use**.
- The trade-off is **cold starts** (first-request delay) — mitigate with **min instances**.
- GCP options: **Cloud Functions** (functions), **Cloud Run** (containers), **App Engine** (apps).
