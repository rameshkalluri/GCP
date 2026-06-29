# GCP Regions, Zones (AZ) & Interview Questions

A reference covering Google Cloud Platform geography (regions, zones, multi-region),
how it maps to AWS terminology, design patterns for high availability, and a curated
set of interview questions with answers.

---

## 1. Core Concepts

### 1.1 Region
A **region** is a specific geographic location where Google hosts its infrastructure
(e.g. `asia-south1` in Mumbai, `us-central1` in Iowa). Each region is an independent
geographic area, and regions are connected over Google's private global network.

- Regions are independent of each other for fault isolation.
- Choosing a region close to your users reduces latency.
- Some Google services are **regional** (data replicated across zones within the region).

### 1.2 Zone (the GCP equivalent of an AWS Availability Zone)
A **zone** is a deployment area *within* a region. Each region has **three or more zones**.
A zone is roughly a single failure domain (think: one or a group of data centers with
independent power, cooling, and networking).

- Naming convention: `<region>-<zone-letter>` → e.g. `asia-south1-a`, `asia-south1-b`, `asia-south1-c`.
- A zone is the smallest deployment target for **zonal** resources (e.g. a single Compute Engine VM, a zonal persistent disk).
- Zone letters (`a`, `b`, `c`) are **per-project mapped** — `us-central1-a` in your project may map to different physical hardware than in another project. Don't assume two projects' "zone a" are the same physical place.

### 1.3 Multi-Region
A **multi-region** is a large geographic area containing two or more regions
(e.g. `us`, `eu`, `asia`). Used by services that replicate data across regions for
the highest durability and availability (e.g. Cloud Storage multi-region buckets,
Spanner, BigQuery).

### 1.4 Resource Scope Summary

| Scope | Example resources | Failure domain |
|---|---|---|
| **Zonal** | Compute Engine VM, zonal persistent disk, zonal GKE node pool | Single zone |
| **Regional** | Regional managed instance group, regional persistent disk, regional GKE cluster, Cloud SQL (HA) | Multiple zones in one region |
| **Multi-regional / Global** | Cloud Storage (multi-region), Cloud Spanner, Cloud DNS, global load balancer, VPC | Multiple regions |

> Key insight: **VPC networks are global** in GCP, but **subnets are regional**.
> This differs from AWS where the VPC is regional.

---

## 2. GCP vs AWS vs Azure Terminology

| Concept | GCP | AWS | Azure |
|---|---|---|---|
| Geographic area | Region | Region | Region |
| Isolated failure domain in a region | Zone | Availability Zone (AZ) | Availability Zone |
| Cross-region grouping | Multi-region | (no direct equivalent) | Geography |
| Global network resource | VPC (global) | VPC (regional) | VNet (regional) |
| Subnet scope | Regional | Per-AZ (zonal) | Regional |
| Edge/CDN locations | Points of Presence (PoPs) | Edge Locations | Edge Locations |

Two notable differences to remember:
1. **GCP subnets span an entire region** (all zones), whereas an **AWS subnet lives in exactly one AZ**.
2. **GCP VPC is global**; AWS VPC is scoped to a single region.

---

## 3. Selected GCP Regions (sample)

> GCP has 40+ regions and 120+ zones (the list keeps growing — always check the
> [official docs](https://cloud.google.com/about/locations) for the current list).

| Region ID | Location | Typical # of zones |
|---|---|---|
| `asia-south1` | Mumbai, India | 3 |
| `asia-south2` | Delhi, India | 3 |
| `asia-southeast1` | Singapore | 3 |
| `asia-east1` | Taiwan | 3 |
| `us-central1` | Iowa, USA | 4 |
| `us-east1` | South Carolina, USA | 3 |
| `us-west1` | Oregon, USA | 3 |
| `europe-west1` | Belgium | 3 |
| `europe-west2` | London, UK | 3 |
| `europe-north1` | Finland | 3 |
| `australia-southeast1` | Sydney | 3 |

Multi-region locations for storage/data services: `us`, `eu`, `asia`.

---

## 4. How to Choose a Region / Zone

Decision factors:
1. **Latency / proximity to users** — pick the closest region.
2. **Compliance & data residency** — some data must stay in a specific country (e.g. India → `asia-south1`/`asia-south2`).
3. **Service availability** — not every product/machine type/GPU exists in every region.
4. **Pricing** — costs vary by region (e.g. `us-central1` is often cheaper than Asia/EU).
5. **Carbon footprint** — Google publishes per-region carbon-free energy percentages.
6. **Quotas** — check resource quotas before committing to a region.

---

## 5. High Availability & Disaster Recovery Patterns

| Goal | Strategy |
|---|---|
| Survive a **single VM** failure | Use a **Managed Instance Group (MIG)** with autohealing |
| Survive a **zone** failure | Deploy across **multiple zones** (regional MIG, regional GKE, regional persistent disk, Cloud SQL HA) |
| Survive a **region** failure | Deploy in **multiple regions** + **Global HTTP(S) Load Balancer** + cross-region data replication (Spanner, multi-region GCS) |
| Global low-latency serving | **Global Load Balancer** with **Cloud CDN** and anycast IP |
| Stateful data durability | **Regional/multi-region** storage; regional persistent disks replicate synchronously across two zones |

Rules of thumb:
- **Spread compute across ≥ 2 zones** for any production workload.
- For RTO/RPO near zero across regions, you need **active-active** with a global LB and a globally-replicated database (e.g. Spanner).
- **Zonal resources are cheapest but least resilient**; reserve them for dev/test or stateless workloads with external state.

---

## 6. Quick Reference: gcloud Commands

```bash
# List all regions
gcloud compute regions list

# List all zones (optionally filter by region)
gcloud compute zones list --filter="region:asia-south1"

# Set default region/zone for the active config
gcloud config set compute/region asia-south1
gcloud config set compute/zone asia-south1-a

# Describe a region (shows quotas + status)
gcloud compute regions describe asia-south1

# Create a VM in a specific zone (zonal resource)
gcloud compute instances create my-vm --zone=asia-south1-a

# Create a regional managed instance group (spans zones)
gcloud compute instance-groups managed create my-mig \
  --region=asia-south1 --template=my-template --size=3
```

---

## 7. Interview Questions & Answers

### Beginner

**Q1. What is the difference between a region and a zone in GCP?**
A region is a geographic area (e.g. `asia-south1`); a zone is an isolated deployment
area within a region (e.g. `asia-south1-a`). A region contains three or more zones,
each acting as an independent failure domain.

**Q2. What is the GCP equivalent of an AWS Availability Zone?**
A **zone**. (And the equivalent of an AWS Region is a GCP **region**.)

**Q3. How are zones named in GCP?**
`<region>-<letter>`, e.g. `us-central1-a`, `europe-west1-b`.

**Q4. Are zone letters consistent across projects?**
No. Zone letters are mapped per-project to physical hardware, so `us-central1-a` may
correspond to different physical infrastructure in two different projects.

**Q5. Name some types of resources by scope.**
- Zonal: a Compute Engine VM, a zonal persistent disk.
- Regional: a regional persistent disk, a regional MIG, Cloud SQL HA.
- Global/Multi-region: VPC, Cloud Storage multi-region bucket, global load balancer, Cloud Spanner.

### Intermediate

**Q6. How does GCP networking scope differ from AWS?**
In GCP, a **VPC is global** and **subnets are regional** (spanning all zones in the
region). In AWS, a VPC is regional and each subnet lives in a single AZ. This means
in GCP a single subnet can host resources across multiple zones.

**Q7. What is a multi-region and which services use it?**
A multi-region (e.g. `us`, `eu`, `asia`) is a large area spanning multiple regions
used for maximum durability/availability. Services: Cloud Storage multi-region
buckets, BigQuery, Cloud Spanner, Cloud DNS.

**Q8. How would you design a service to survive a zone outage?**
Deploy compute across at least two zones using a **regional managed instance group**
(or regional GKE cluster), front it with a load balancer, and use regional/replicated
storage (regional persistent disk or Cloud SQL HA) so no single zone is a SPOF.

**Q9. How would you survive a full region outage?**
Run in multiple regions behind a **Global HTTP(S) Load Balancer** (single anycast IP),
replicate data across regions (Cloud Spanner or multi-region Cloud Storage), and use
Cloud DNS / health checks for failover. This is an active-active or active-passive
multi-region architecture.

**Q10. What factors influence region selection?**
Latency to users, data-residency/compliance, service & machine-type availability,
pricing, carbon-free energy %, and resource quotas.

### Advanced

**Q11. What's the difference between a regional and a zonal persistent disk?**
A zonal PD lives in one zone; a regional PD synchronously replicates data across **two
zones** in the same region, allowing failover if one zone fails (used for HA stateful
workloads).

**Q12. How does a Global Load Balancer differ from a regional one?**
A global external HTTP(S) LB uses a single anycast IP and routes users to the nearest
healthy backend in **any region**, enabling cross-region failover and lower latency.
A regional LB serves backends within a single region only.

**Q13. Is a GKE cluster zonal or regional? Why does it matter?**
It can be either. A **zonal** cluster has its control plane (and optionally nodes) in
one zone — control plane is a SPOF. A **regional** cluster replicates the control plane
across three zones and can spread nodes across zones, giving higher availability and
zero-downtime control-plane upgrades.

**Q14. If you need a globally consistent, horizontally scalable relational database, what do you pick?**
**Cloud Spanner** — it provides global, strongly-consistent, horizontally-scalable
SQL with multi-region replication. (Cloud SQL is regional with HA across zones, not
globally distributed.)

**Q15. Why might "zone a" being available in your project not help a colleague in another project?**
Because zone letters are independently mapped per project. Capacity or an outage tied
to a physical zone may surface under different letters in different projects, so
coordinating on letters across projects is meaningless.

**Q16. What is the blast radius of a subnet in GCP, and how does that affect HA design?**
A GCP subnet spans an entire region (all zones). So zone-level HA does **not** require
multiple subnets — you can place VMs from the same subnet into different zones. You
only need multiple subnets for multiple regions or network segmentation.

---

## 8. One-Line Cheat Sheet

- **Region** = geographic area; **Zone** = failure domain inside a region (≥ 3 per region).
- **Zone = AWS AZ.**
- **GCP VPC = global; subnet = regional. (AWS VPC/subnet = regional/zonal.)**
- **Zonal → Regional → Multi-region/Global** = increasing resilience & cost.
- **2+ zones** for prod; **2+ regions + global LB** for region-level DR.
