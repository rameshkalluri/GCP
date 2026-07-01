# Day 7 — Serverless (Functions, Cloud Run, App Engine)

Notes for Day 7 of the [10-Day GCP Learning Plan](../GCP-10-Day-Learning-Plan.md).

> **Big-picture analogy:** Serverless is like **electricity from a socket**. You don't
> build a power plant (servers) — you just **plug in and pay for what you use**. When
> nothing is plugged in, you pay (almost) nothing. Google runs the "power plant"; you
> only care about your **appliance (code)**.

## Topics
1. [Serverless Fundamentals](01-serverless-fundamentals.md) — what it means, scale-to-zero, pricing, cold starts.
2. [Cloud Functions](02-cloud-functions.md) — tiny event-driven functions (FaaS).
3. [Cloud Run](03-cloud-run.md) — run any container, serverless.
4. [App Engine](04-app-engine.md) — the original managed app platform (PaaS).
5. [Choosing & Cold Starts](05-choosing-and-cold-starts.md) — pick the right one; manage cold starts.

## Day 7 Goals
- [ ] Explain what "serverless" means and the scale-to-zero pricing model.
- [ ] Know the difference between Cloud Functions, Cloud Run, and App Engine.
- [ ] Understand cold starts and how to reduce them.
- [ ] Deploy an HTTP Cloud Function.
- [ ] Containerize a small app and deploy it to Cloud Run.
