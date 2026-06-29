# Cloud Fundamentals

## What is Cloud Computing?
On-demand delivery of computing resources (servers, storage, databases, networking,
software) over the internet, with **pay-as-you-go** pricing — no need to buy/manage
physical hardware.

### Key benefits
- **No upfront capex** — rent instead of buy.
- **Elasticity** — scale up/down with demand.
- **Global reach** — deploy close to users in minutes.
- **Managed services** — Google handles undifferentiated heavy lifting.
- **Reliability** — built-in redundancy across zones/regions.

---

## Service Models (IaaS / PaaS / SaaS)

| Model | You manage | Provider manages | GCP examples |
|---|---|---|---|
| **IaaS** | OS, runtime, apps, data | Hardware, virtualization, network | Compute Engine, VPC, Persistent Disk |
| **PaaS** | Apps, data | OS, runtime, scaling, hardware | App Engine, Cloud Run, Cloud SQL |
| **SaaS** | Just use it | Everything | Google Workspace, Gmail |
| **FaaS** (serverless) | Just your function code | Servers, scaling, infra | Cloud Functions |

> Rule of thumb: moving IaaS → PaaS → SaaS means **less control but less ops burden**.

---

## Deployment Models
- **Public cloud** — resources owned by the provider (GCP, AWS, Azure).
- **Private cloud** — dedicated to one organization (on-prem or hosted).
- **Hybrid cloud** — mix of on-prem + cloud (e.g., GCP **Anthos**).
- **Multi-cloud** — using more than one cloud provider.

---

## What is GCP?
Google Cloud Platform — Google's suite of cloud services running on the **same
infrastructure** Google uses for Search, Gmail, and YouTube.

### Main product categories
- **Compute:** Compute Engine, App Engine, Cloud Run, Cloud Functions, GKE.
- **Storage:** Cloud Storage, Persistent Disk, Filestore.
- **Databases:** Cloud SQL, Firestore, Bigtable, Spanner, Memorystore.
- **Networking:** VPC, Cloud Load Balancing, Cloud CDN, Cloud DNS.
- **Big Data / AI:** BigQuery, Dataflow, Pub/Sub, Vertex AI.
- **Operations:** Cloud Monitoring, Logging, Error Reporting.

---

## How You Interact with GCP
- **Cloud Console** — web UI.
- **gcloud CLI / Cloud SDK** — command line.
- **Cloud Shell** — browser-based terminal with tools pre-installed.
- **Client libraries** — for Python, Java, Go, Node.js, etc.
- **REST / gRPC APIs** — programmatic access.

---

## TL;DR
- Cloud = rent computing on-demand, pay for what you use.
- **IaaS/PaaS/SaaS** = decreasing control, decreasing ops effort.
- GCP runs on Google's global infrastructure with compute, storage, DB, networking,
  data/AI, and ops services.
- Interact via Console, gcloud CLI, Cloud Shell, client libraries, or APIs.
