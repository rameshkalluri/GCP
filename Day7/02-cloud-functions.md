# Cloud Functions

## What is Cloud Functions?
**Cloud Functions** is GCP's **Functions-as-a-Service (FaaS)** — you write a **single
small function**, and it runs in response to an **event**, with no servers to manage.

> **Analogy:** A **motion-sensor light**. You don't run it 24/7 — it **triggers the
> instant something happens** (motion), does one job (turn on), then goes quiet.

---

## Event-driven: what can trigger a function?
- **HTTP request** — call it like an API endpoint.
- **Cloud Storage** event — a file is uploaded/deleted (e.g., make a thumbnail).
- **Pub/Sub** message — a message arrives on a topic.
- **Firestore** change — a document is created/updated.
- **Scheduled** (via Cloud Scheduler) — run on a cron schedule.

> **Analogy:** Different **sensors** trip the same kind of light: a doorbell, a
> motion sensor, a timer — each an "event" that runs your function.

---

## Great use cases ✅
- **Glue code** between services (when X happens, do Y).
- Image/file processing on upload.
- Lightweight webhooks and small APIs.
- Reacting to Pub/Sub messages / database changes.

## Not great for ❌
- Long-running jobs or heavy, always-on services (use **Cloud Run**/GKE).
- Complex apps needing full control over the runtime/container.

---

## Key concepts
- **Stateless** — each invocation is independent; store state in a DB/bucket.
- **Auto-scales** per event volume (and **scales to zero**).
- **Timeout limits** — functions are meant to be **short-lived**.
- **2nd gen** Cloud Functions run on **Cloud Run** under the hood (more power, concurrency).

---

## Quick `gcloud` Reference
```bash
# Deploy an HTTP-triggered function (2nd gen), Python example
gcloud functions deploy hello-http \
  --gen2 --runtime=python312 --region=asia-south1 \
  --source=. --entry-point=hello --trigger-http --allow-unauthenticated

# Deploy a function triggered by a Cloud Storage upload
gcloud functions deploy on-upload \
  --gen2 --runtime=python312 --region=asia-south1 \
  --source=. --entry-point=process --trigger-bucket=my-bucket
```

```python
# main.py (entry-point "hello")
import functions_framework

@functions_framework.http
def hello(request):
    return "Hello from Cloud Functions!"
```

---

## TL;DR (in plain English)
- **Cloud Functions = motion-sensor light**: a small function that runs on an **event**
  (HTTP, Storage, Pub/Sub, Firestore, schedule).
- **Stateless, auto-scaling, scales to zero**; meant for **short** tasks.
- Perfect for **glue code, webhooks, file processing**; use **Cloud Run** for full apps.
