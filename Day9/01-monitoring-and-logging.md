# Monitoring & Logging

Google's operations suite (formerly **Stackdriver**) is your **control room** for
watching everything running in GCP.

> **Analogy:** A building's **security control room**: **cameras** record what happened
> (logs), **dashboards/gauges** show live health (metrics), an **alarm** rings when
> something's wrong (alerts), and an **incident board** flags recurring faults (errors).

---

## Cloud Monitoring — the live gauges 📊
Collects **metrics** (numbers over time) about your resources: CPU, memory, request
count, latency, error rate, etc.

- **Dashboards** — visualize metrics (graphs/charts).
- **Uptime checks** — periodically ping your service from around the world ("is it up?").
- **Alerting policies** — notify you when a metric crosses a threshold.
- Works across VMs, GKE, Cloud Run, load balancers, databases, and more.

> **Analogy:** the **speedometer and fuel gauge** of your infrastructure — live numbers
> telling you how things are running right now.

---

## Cloud Logging — the recorded footage 🎥
Collects, stores, and lets you **search logs** from all your services.

- **Log Explorer** — query and filter logs.
- **Log-based metrics** — turn log patterns into metrics (e.g., count of `ERROR` lines).
- **Log sinks** — route/export logs to **BigQuery**, **Cloud Storage**, or **Pub/Sub**
  for analysis or long-term retention.
- Includes **audit logs** (Day 2: who did what, when).

```bash
# Read recent logs
gcloud logging read "severity>=ERROR" --limit=20

# Create a sink exporting logs to BigQuery
gcloud logging sinks create my-sink \
  bigquery.googleapis.com/projects/PROJECT/datasets/logs \
  --log-filter='severity>=WARNING'
```

---

## Error Reporting — the incident board 🚨
**Automatically groups and counts** application errors/exceptions, so you see **which
errors matter most** and when they started — instead of scrolling raw logs.

> **Analogy:** Instead of watching hours of camera footage, a board says: "**This exact
> fault happened 480 times, starting 2pm.**"

---

## Cloud Trace & Profiler — performance detectives 🔍
- **Cloud Trace** — **distributed tracing**: see how long a request spends in each
  service/hop (find the slow step).
  - *Analogy:* a **timeline of a package's journey** showing where it got delayed.
- **Cloud Profiler** — continuously samples CPU/memory to show which **code** is expensive.

---

## The four together (mental model)
```
Metrics (Monitoring) → live health & gauges
Logs (Logging)       → detailed recorded events (searchable)
Errors (Error Rep.)  → grouped app exceptions
Traces (Trace)       → where time goes across services
```

---

## TL;DR (in plain English)
- **Cloud Monitoring = live gauges** (metrics, dashboards, uptime checks, alerts).
- **Cloud Logging = recorded footage** (searchable logs; export via **sinks** to
  BigQuery/Storage/Pub/Sub; includes audit logs).
- **Error Reporting = incident board** (auto-groups app errors).
- **Cloud Trace/Profiler** = find **slow requests** and **expensive code**.
