# App Engine

## What is App Engine?
**App Engine** is GCP's original **Platform-as-a-Service (PaaS)** — you deploy your
**application code** and Google handles servers, scaling, load balancing, and health.

> **Analogy:** A **fully managed apartment**. You bring your **furniture (code)** and
> move in; the landlord handles plumbing, electricity, security, and repairs (infra,
> scaling, patching). You never touch the building's machinery.

---

## Standard vs Flexible environment

| | **Standard** | **Flexible** |
|---|---|---|
| Runs on | Google's sandbox (specific runtimes) | Containers on **Compute Engine** VMs |
| Scaling | Very fast, **scales to zero** | Slower, min 1 instance (no scale-to-zero) |
| Runtimes | Preset languages/versions | Any (custom Docker) |
| Cold starts | Possible (fast) | Fewer (always warm) |
| Analogy | A **studio apartment** — quick, limited customization | A **house** — more control, more cost |

---

## Key features
- **Automatic scaling** based on traffic.
- **Versions & traffic splitting** — deploy a new version and send it, say, **10%** of
  traffic (canary / A/B).
  - *Analogy:* let a **few customers** try the new menu before rolling it out to all.
- **Built-in services** — load balancing, health checks, logging.
- Deploy with a simple `app.yaml` config.

---

## App Engine vs Cloud Run (they overlap)
- **App Engine** — the older PaaS; deploy **code** (Standard) with an `app.yaml`.
- **Cloud Run** — newer, container-first, more portable, scales to zero with concurrency.
- 👉 For **new projects**, many teams now prefer **Cloud Run**; App Engine remains great
  for classic web apps and existing deployments.

---

## When to use App Engine ✅
- Traditional web apps/APIs you want fully managed with minimal config.
- You like the `app.yaml` workflow, versions, and traffic splitting.

---

## Quick `gcloud` Reference
```bash
# One-time: create the App Engine app in a region
gcloud app create --region=asia-south1

# Deploy (reads app.yaml in the current directory)
gcloud app deploy

# Open it in the browser / view logs
gcloud app browse
gcloud app logs tail -s default
```

```yaml
# app.yaml (Standard, Python example)
runtime: python312
instance_class: F1
automatic_scaling:
  min_instances: 0
  max_instances: 10
```

---

## TL;DR (in plain English)
- **App Engine = fully managed PaaS** — deploy your app; Google runs/scales/patches it.
- **Standard** = fast, scales to zero, preset runtimes; **Flexible** = containers on VMs,
  more control, always warm.
- Offers **versions + traffic splitting** for safe rollouts; for new container apps,
  **Cloud Run** is often the modern pick.
