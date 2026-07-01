# GKE Basics

## What is GKE?
**Google Kubernetes Engine (GKE)** is Google's **managed Kubernetes** service. Google
runs and maintains the **control plane** (and optionally the nodes), so you get
Kubernetes without building it from scratch.

> **Analogy:** Kubernetes is a **complex port**. Running it yourself means building and
> staffing the whole port. **GKE = Google runs the port for you** — you just send your
> containers and say how many you want.

---

## What GKE manages for you
- **Control plane** (API server, scheduler, etcd) — highly available, auto-upgraded.
- **Node provisioning, upgrades, repair** (especially in Autopilot).
- **Integration** with GCP: load balancing, IAM, logging/monitoring, VPC, Artifact Registry.

---

## Autopilot vs Standard (the big choice)

| | **Autopilot** | **Standard** |
|---|---|---|
| You manage | Just workloads (Pods) | Nodes **and** workloads |
| Nodes | Fully managed by Google | You configure/size node pools |
| Billing | **Per Pod** resource usage | **Per node** (VMs), even if idle |
| Control | Less (opinionated, safer) | More (full node access, DaemonSets, GPUs, custom) |
| Best for | Most teams; hands-off ops | Special needs, fine-grained control |
| Analogy | **Fully-serviced port** — just ship cargo | **You run the cranes** — more control, more work |

> **Rule of thumb:** start with **Autopilot** unless you need node-level control
> (special hardware, custom OS, DaemonSets, tight cost tuning on node packing).

---

## Control plane vs nodes vs node pools
- **Control plane** — the brain (Google-managed in GKE).
- **Node** — a worker VM (Compute Engine) that runs your Pods.
- **Node pool** — a group of nodes with the **same config** (e.g., a pool of GPU nodes).
  - *Analogy:* different **teams of dock workers** with different equipment.

---

## Zonal vs Regional cluster (HA)
- **Zonal** — control plane in **one zone** (a single point of failure).
- **Regional** — control plane **replicated across zones** + nodes spread across zones →
  survives a zone outage. **Preferred for production.** (Ties back to Day 3 HA.)

---

## Quick `gcloud` Reference
```bash
# Create an Autopilot cluster (simplest)
gcloud container clusters create-auto my-cluster --region=asia-south1

# Create a Standard cluster with a node pool
gcloud container clusters create my-std-cluster \
  --zone=asia-south1-a --num-nodes=3 --machine-type=e2-medium

# Get credentials so kubectl can talk to the cluster
gcloud container clusters get-credentials my-cluster --region=asia-south1

# Delete the cluster (stop paying!) — important after labs
gcloud container clusters delete my-cluster --region=asia-south1
```

---

## TL;DR (in plain English)
- **GKE = managed Kubernetes**: Google runs the control plane (and repairs/upgrades),
  you deploy containers.
- **Autopilot** = hands-off, pay-per-Pod, less control (**start here**); **Standard** =
  you manage nodes, more control, pay-per-node.
- Use a **regional cluster** for production HA; always **delete clusters** after labs
  to avoid charges.
