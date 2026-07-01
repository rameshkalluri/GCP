# Cloud Run

## What is Cloud Run?
**Cloud Run** runs your **container** as a serverless service — **any language, any
library**, packaged in a container image. It **auto-scales** (including **to zero**)
and you **pay per use**.

> **Analogy:** If Cloud Functions is a **single motion-sensor light**, Cloud Run is a
> **fully furnished pop-up shop in a box**. You bring your **container** (the whole
> shop, set up your way), and Google opens/closes it based on demand.

---

## Why Cloud Run is popular
- **Bring any container** — full control of language, runtime, dependencies.
- **Serverless** — no cluster to manage (unlike GKE), scales to zero.
- **Fast autoscaling** — handles spikes automatically.
- **Concurrency** — one instance can serve **many requests at once** (cheaper than FaaS).
- **HTTPS + custom domains** out of the box.

---

## How it works (simple)
```
Your code → Docker image → push to Artifact Registry → deploy to Cloud Run → HTTPS URL
```
- You give it a **container image**; Cloud Run runs and scales it.
- It listens on the port from the `$PORT` environment variable.

---

## Cloud Run vs Cloud Functions
| | **Cloud Functions** | **Cloud Run** |
|---|---|---|
| Unit | A single function | A whole **container** |
| Control | Limited (managed runtimes) | Full (any language/OS libs) |
| Concurrency | ~1 request per instance (gen1) | **Many** requests per instance |
| Best for | Event glue, small tasks | Web apps, APIs, microservices |

> Since Cloud Functions (2nd gen) runs **on** Cloud Run, think of Cloud Run as the more
> flexible, general-purpose serverless container platform.

---

## Cloud Run vs GKE
- **Cloud Run** — serverless containers, **no cluster to manage**, scales to zero.
- **GKE (Day 8)** — full Kubernetes, more control/features, you manage the cluster.
- *Analogy:* Cloud Run = **renting a fully-serviced pop-up**; GKE = **running your own
  shopping mall** (more power, more responsibility).

---

## When to use Cloud Run ✅
- Web apps, REST/gRPC APIs, microservices.
- You already have a **Dockerfile**/container.
- You want serverless scaling without Kubernetes complexity.

---

## Quick `gcloud` Reference
```bash
# Build & deploy from source (Cloud Run builds the container for you)
gcloud run deploy my-app \
  --source=. --region=asia-south1 --allow-unauthenticated

# Or deploy a prebuilt image
gcloud run deploy my-app \
  --image=asia-south1-docker.pkg.dev/PROJECT/repo/my-app:latest \
  --region=asia-south1 --allow-unauthenticated

# Keep 1 instance warm (reduce cold starts)
gcloud run services update my-app --region=asia-south1 --min-instances=1
```

---

## TL;DR (in plain English)
- **Cloud Run = serverless containers** — bring any container, it auto-scales (to zero),
  pay per use, HTTPS included.
- More flexible than Cloud Functions (**any language, high concurrency**); simpler than
  GKE (**no cluster to manage**).
- Ideal for **web apps, APIs, and microservices**; reduce cold starts with **min-instances**.
