# GCP in 10 Days — Learning Plan

A focused, hands-on plan to go from zero to job-ready fundamentals on Google Cloud Platform. Budget **3–4 hours/day** (≈1 hr theory + 2–3 hrs hands-on labs).

> Tip: Use a **free-tier GCP account** ($300 credit for 90 days). Always set a **budget alert** and **delete resources** after each lab to avoid charges.

---

## Prerequisites (Day 0 — Setup)
- Create a Google Cloud account and claim free credits.
- Install the [gcloud CLI](https://cloud.google.com/sdk/docs/install) and run `gcloud init`.
- Enable billing + set a **budget alert** at $10.
- Bookmark: [Cloud Console](https://console.cloud.google.com), [GCP Docs](https://cloud.google.com/docs), [Google Cloud Skills Boost](https://www.cloudskillsboost.google).

---

## Day 1 — Cloud Fundamentals & Account Structure
**Theory**
- What is cloud computing (IaaS/PaaS/SaaS), regions & zones.
- GCP resource hierarchy: Organization → Folder → Project → Resources.
- Billing, IAM basics, the Cloud Console & Cloud Shell.

**Hands-on**
- Create a project, explore Cloud Shell.
- Run your first `gcloud` commands (`gcloud config list`, `gcloud projects list`).

---

## Day 2 — IAM & Security Basics
**Theory**
- IAM: members, roles (primitive, predefined, custom), policies.
- Service accounts & keys, principle of least privilege.

**Hands-on**
- Create a service account, assign a role.
- Grant/revoke a role to a user. Inspect policy with `gcloud projects get-iam-policy`.

---

## Day 3 — Compute Engine (VMs)
**Theory**
- Machine types, images, persistent disks, preemptible/Spot VMs.
- Instance metadata, startup scripts, instance groups.

**Hands-on**
- Launch a Linux VM, SSH in, install a web server.
- Create a managed instance group + autoscaling. **Delete when done.**

---

## Day 4 — Networking (VPC)
**Theory**
- VPC, subnets, firewall rules, routes.
- External vs internal IPs, Cloud NAT, load balancing overview.

**Hands-on**
- Create a custom VPC + subnet + firewall rules.
- Set up an HTTP(S) load balancer in front of 2 VMs.

---

## Day 5 — Storage Options
**Theory**
- Cloud Storage (buckets, storage classes, lifecycle).
- Persistent Disks vs Filestore. When to use what.

**Hands-on**
- Create a bucket, upload/download objects, set lifecycle rules.
- Host a static website on Cloud Storage.

---

## Day 6 — Databases
**Theory**
- Cloud SQL (MySQL/Postgres), Firestore, Bigtable, Spanner — use cases.
- Managed vs self-managed trade-offs.

**Hands-on**
- Spin up a Cloud SQL instance, connect from Cloud Shell.
- Create a Firestore collection and add documents.

---

## Day 7 — Serverless (Functions, Cloud Run, App Engine)
**Theory**
- Cloud Functions (event-driven), Cloud Run (containers), App Engine.
- Cold starts, scaling to zero, pricing model.

**Hands-on**
- Deploy a Cloud Function (HTTP trigger).
- Containerize a small app and deploy it to Cloud Run.

---

## Day 8 — Containers & Kubernetes (GKE)
**Theory**
- Docker basics, Kubernetes concepts (pods, deployments, services).
- GKE Autopilot vs Standard.

**Hands-on**
- Create a GKE cluster, deploy a sample app, expose via service.
- Scale a deployment. **Delete the cluster when done.**

---

## Day 9 — Operations, IaC & Data/AI Intro
**Theory**
- Cloud Monitoring, Logging, Error Reporting.
- Infrastructure as Code: Terraform / Deployment Manager.
- Quick tour: BigQuery (analytics) & Vertex AI (ML).

**Hands-on**
- View logs/metrics dashboards for a running service.
- Provision a VM with Terraform.
- Run a sample query in BigQuery public datasets.

---

## Day 10 — Capstone Project + Review
**Build a small end-to-end app**, e.g.:
- Frontend on Cloud Run or Cloud Storage (static).
- Backend API on Cloud Run / Cloud Functions.
- Cloud SQL or Firestore database.
- VPC + IAM least-privilege + budget alert.
- Logging/monitoring enabled.

**Review & Next Steps**
- Revisit weak areas.
- Consider certification: **Associate Cloud Engineer** (good first cert).
- Tear down all billable resources.

---

## Recommended Resources
- **Google Cloud Skills Boost** (hands-on labs/quests) — primary practice.
- **Official Docs & Tutorials** — `cloud.google.com/docs`.
- **YouTube**: "Google Cloud Tech" channel.
- **Cert prep**: *Associate Cloud Engineer* study guide + practice exams.

## Daily Habit Checklist
- [ ] 1 hr concept reading
- [ ] 2–3 hrs hands-on lab
- [ ] Note key `gcloud` commands learned
- [ ] **Delete/stop billable resources**
- [ ] Check billing dashboard
