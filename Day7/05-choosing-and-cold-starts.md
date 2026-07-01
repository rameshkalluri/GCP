# Choosing a Serverless Option & Managing Cold Starts

## Quick decision guide 🧭

```
Is your workload a single, event-triggered task (file upload, message, webhook)?
   └── YES → Cloud Functions

Do you have a container / need any language + full control, as a web app or API?
   └── YES → Cloud Run   (modern default for containers)

Do you want a classic managed app platform with app.yaml, versions & traffic splitting?
   └── YES → App Engine

Do you need full Kubernetes control, custom networking, or many coordinated services?
   └── YES → GKE (Day 8) — not serverless, but maximum control
```

---

## Side-by-side

| | **Cloud Functions** | **Cloud Run** | **App Engine** |
|---|---|---|---|
| Deploy unit | Function | Container | App code (`app.yaml`) |
| Language | Preset runtimes | **Any** (container) | Preset (Standard) / any (Flexible) |
| Scale to zero | ✅ | ✅ | ✅ (Standard) |
| Concurrency | Low (per-instance) | **High** | Medium |
| Best for | Event glue, webhooks | Web apps, APIs, microservices | Classic web apps |
| Analogy | Motion-sensor light | Pop-up shop in a box | Managed apartment |

---

## Pricing model (all three)
- Billed by **requests + compute time + memory** used, with **scale-to-zero** meaning
  ~no cost when idle.
- *Analogy:* a **taxi meter** that only runs during the ride — parked = free.
- Watch: **network egress**, **min-instances** (kept-warm instances cost money even idle).

---

## Cold Starts — deeper look 🥶

A **cold start** is the delay when a **new instance** must boot to handle a request
(because the service scaled to zero or is scaling up).

> **Analogy:** Opening a **closed shop** — shutters up, lights on, till ready. The
> first customer waits; the rest are served instantly (warm).

### How to reduce cold starts
- **Minimum instances** — keep 1+ instances **always warm** (Cloud Run/App Engine).
- **Smaller images / lighter runtimes** — less to load = faster boot.
- **Lazy-load** heavy dependencies; keep global init light.
- **Keep instances warm** with a scheduled ping (Cloud Scheduler) if needed.
- Choose **higher concurrency** (Cloud Run) so fewer new instances are needed.

> Trade-off: **min-instances removes cold starts but costs money** even when idle —
> balance latency needs vs cost.

---

## TL;DR (in plain English)
- **Cloud Functions** for events/glue, **Cloud Run** for containers/apps (modern
  default), **App Engine** for classic managed apps, **GKE** when you need full Kubernetes.
- All are **pay-per-use** and **scale to zero** (watch egress + kept-warm instances).
- **Cold start** = first-request boot delay; reduce with **min-instances**, small images,
  light init, and higher concurrency.
