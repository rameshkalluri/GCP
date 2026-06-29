# Load Balancing

## What is a Load Balancer?
A **load balancer (LB)** spreads incoming traffic across **multiple VMs** so no single
machine gets overwhelmed.

> **Analogy:** Picture a **busy supermarket**. Instead of forcing everyone into **one
> checkout line**, a **traffic manager (the load balancer)** sends each customer to
> whichever **cashier (VM)** is free. Faster for customers, fairer for cashiers.

---

## Why use one?
- **Handle more traffic** — many VMs share the work.
  - *Analogy:* open more checkout lanes at rush hour.
- **No single point of failure** — if one VM dies, traffic goes to the others.
  - *Analogy:* if one cashier goes on break, customers still get served.
- **Health checks** — the LB stops sending traffic to **unhealthy** VMs.
  - *Analogy:* the manager won't send customers to a **closed** lane.
- **Scales with autoscaling** — pairs perfectly with a **MIG** (Day 3) that adds/removes VMs.
- **One front door** — users hit a single IP, not individual VMs.

---

## Key Building Blocks (simple view)
```
User → [ Forwarding Rule + IP ] → [ Backend Service ] → [ Backend VMs / MIG ]
                                          ↑
                                   [ Health Check ]
```
- **Frontend** — the IP/port users connect to (the **front door**).
- **Backend service** — the group of VMs (often a **MIG**) doing the work (the **cashiers**).
- **Health check** — pings backends to see who's healthy (the **manager checking lanes**).

---

## Types of Load Balancers (don't memorize, just understand)

### Global vs Regional
- **Global LB** — one IP serving users **worldwide**, routing each user to the **nearest**
  healthy region.
  - *Analogy:* a **global hotline** that auto-connects you to the **closest branch**.
- **Regional LB** — serves backends within **one region**.

### External vs Internal
- **External LB** — faces the **internet** (public users).
  - *Analogy:* the **public entrance** of the store.
- **Internal LB** — serves traffic **inside your VPC** (e.g., app servers → internal API).
  - *Analogy:* an **employees-only internal help desk**.

### By traffic type
- **Application LB (HTTP/HTTPS)** — understands web traffic; can route by **URL/path**
  (e.g., `/images` → image servers, `/api` → API servers).
  - *Analogy:* a smart receptionist who **reads your request** and sends you to the
    right department.
- **Network LB (TCP/UDP)** — fast, lower-level routing; doesn't read the content.
  - *Analogy:* a guard who just **points you to a lane** without asking why.

### Common pick
- **Public website/app** → **Global External Application (HTTP/S) Load Balancer**, often
  with **Cloud CDN** for caching.

---

## Bonus friends of the LB
- **Cloud CDN** — caches content close to users for speed.
  - *Analogy:* keeping **popular items near the entrance** so people grab them fast.
- **Health checks** — auto-remove bad VMs from rotation.
- **SSL/HTTPS** — the LB can handle certificates (secure padlock) for you.

---

## Quick `gcloud` Reference (high level)
```bash
# Health check (manager checks if a lane is open)
gcloud compute health-checks create http my-hc --port=80

# Backend service + attach a managed instance group of VMs
gcloud compute backend-services create my-backend \
  --protocol=HTTP --health-checks=my-hc --global
gcloud compute backend-services add-backend my-backend \
  --instance-group=web-mig --instance-group-region=asia-south1 --global

# URL map + proxy + forwarding rule (the public front door)
gcloud compute url-maps create my-map --default-service=my-backend
gcloud compute target-http-proxies create my-proxy --url-map=my-map
gcloud compute forwarding-rules create my-rule \
  --global --target-http-proxy=my-proxy --ports=80
```

---

## TL;DR (in plain English)
- A **load balancer = the traffic manager** sending users to free, healthy VMs.
- Benefits: **handles more load, survives VM failures, health checks, one front door.**
- Flavors: **Global vs Regional**, **External vs Internal**, **Application (HTTP, smart)
  vs Network (TCP/UDP, fast)**.
- Common setup: **Global External HTTP(S) LB + MIG + health checks (+ Cloud CDN)**.
