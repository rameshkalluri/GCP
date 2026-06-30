# GCP Day 4 — Interview Questions & Answers

A curated set of interview questions with answers covering **Day 4 — Networking (VPC)**:
VPC & subnets, firewall rules & routes, IP addresses & Cloud NAT, load balancing, and
DNS/CDN/connectivity.

---

## VPC & Subnets

**Q1. What is a VPC?**
A **Virtual Private Cloud** is your own private network inside Google Cloud where VMs and services live and communicate — like a gated private city with private roads, controlled by firewall "gates."

**Q2. What is the key difference between a GCP VPC and an AWS VPC?**
A **GCP VPC is global** — one VPC can span every region. An **AWS VPC is regional** (scoped to a single region).

**Q3. What is a subnet and what is its scope in GCP?**
A subnet is a slice of the VPC's address space with its own IP range (CIDR). In GCP, **subnets are regional** — a subnet spans **all zones** in its region. (In AWS, a subnet lives in a single AZ.)

**Q4. Explain CIDR notation like `10.0.1.0/24`.**
It defines a block of IP addresses. The number after `/` is the prefix length: a **smaller number = a bigger range**. `/16` ≈ 65,000 addresses; `/24` ≈ 256 addresses. Use private ranges (`10.x`, `172.16–31.x`, `192.168.x`) for internal IPs.

**Q5. Default vs custom VPC (and auto vs custom mode)?**
A **default VPC** is auto-created with subnets in every region + basic firewall rules (good for learning). A **custom VPC** lets you design subnets/ranges yourself (recommended for production). **Auto mode** creates subnets automatically per region; **custom mode** means you choose them.

**Q6. How do VMs in different VPCs communicate?**
VMs in the **same VPC** talk over internal IPs (even across regions, subject to firewall rules). To connect **two different VPCs**, use **VPC Peering** or a **Shared VPC**.

---

## Firewall Rules & Routes

**Q7. What does a firewall rule do, and what are its components?**
It allows or denies traffic. Components: **direction** (ingress/egress), **action** (allow/deny), **target** (which VMs, via tags/SA), **source/destination** (IP ranges), **protocol & port** (e.g., TCP:80, TCP:22), and **priority** (lower number checked first / wins).

**Q8. What are the default firewall behaviors in GCP?**
By default, **all incoming (ingress) traffic is blocked** and **all outgoing (egress) traffic is allowed**. You open specific doors by adding allow rules.

**Q9. How does firewall rule priority work?**
Rules are evaluated from **lowest number (0)** to **highest (65535)**, and the **first match wins**. A lower-priority-number rule overrides higher-numbered ones.

**Q10. What are network tags and why use them?**
A network tag is a label on a VM (e.g., `web-server`). Firewall rules can target tags ("allow port 80 to all VMs tagged `web-server`"), making it easy to apply rules to groups of VMs.

**Q11. What is a route, and how is it different from a firewall rule?**
A **route** decides **which path** traffic takes to a destination (road signs/GPS). A **firewall rule** decides **whether** traffic is allowed (security gate). Both cooperate: a route gets the traffic to the gate; the firewall decides if it passes. GCP auto-creates default routes (VM-to-VM within the VPC, and a default internet route `0.0.0.0/0`).

**Q12. What does `0.0.0.0/0` mean and when is it risky?**
It means "from anywhere on the internet." Fine for public web ports (80/443), but for **SSH (port 22)** you should restrict it to your own IP for safety.

---

## IP Addresses & Cloud NAT

**Q13. Internal vs external IP?**
**Internal (private) IP** is reachable only inside the VPC (assigned to every VM automatically). **External (public) IP** is reachable from the internet and is **optional** — only needed if the VM must be reached from outside (e.g., a public web server).

**Q14. Static vs ephemeral IP?**
**Ephemeral** IPs are assigned automatically and can change when the VM stops/starts (fine for temporary VMs). **Static** IPs are reserved and never change (needed for production servers, DNS records, whitelists). Note: a **reserved static IP that's not attached still costs money** — release it if unused.

**Q15. What is Cloud NAT and what problem does it solve?**
A VM with only an internal IP can't normally reach the internet, but may need to download updates or call external APIs. **Cloud NAT** lets such private VMs make **outbound** internet requests **without** a public IP — like staff calling out through a building switchboard. Outsiders can't initiate connections in.

**Q16. Why is Cloud NAT good for security?**
VMs stay hidden from the internet (no inbound exposure) while still being able to pull updates / call external APIs (outbound only) — like one-way glass: you can see out, no one can see in.

**Q17. Quick decision guide for IP/NAT?**
- VM only talks to other VMs privately → **internal IP only**.
- VM must be reached from the internet → add an **external IP** (static if it must be fixed).
- Private VM needs outbound internet but stays hidden → **Cloud NAT**.

---

## Load Balancing

**Q18. What is a load balancer and why use one?**
A load balancer spreads incoming traffic across multiple VMs so none is overwhelmed. Benefits: handle more traffic, no single point of failure, health checks (skip unhealthy VMs), pairs with autoscaling MIGs, and gives users **one front door** (single IP).

**Q19. What are the main building blocks of a GCP load balancer?**
**Frontend** (forwarding rule + IP/port — the front door), **backend service** (the group of VMs, often a MIG), and a **health check** (pings backends to route only to healthy ones).

**Q20. Global vs regional load balancer?**
A **global LB** uses one IP serving users worldwide, routing each to the nearest healthy region. A **regional LB** serves backends within a single region.

**Q21. External vs internal load balancer?**
**External LB** faces the internet (public users). **Internal LB** serves traffic inside your VPC (e.g., app servers calling an internal API).

**Q22. Application (HTTP/HTTPS) LB vs Network (TCP/UDP) LB?**
An **Application LB** understands web traffic and can route by **URL/path** (`/images` → image servers, `/api` → API servers) — a smart receptionist. A **Network LB** does fast, lower-level TCP/UDP routing without reading content — a guard who just points you to a lane.

**Q23. What's a common setup for a public website?**
A **Global External Application (HTTP/S) Load Balancer** + a **MIG** of backends + **health checks**, often with **Cloud CDN** for caching and SSL/HTTPS handled at the LB.

---

## DNS, CDN & Connectivity

**Q24. What is Cloud DNS?**
Google's managed DNS service. **DNS** translates names (`www.myshop.com`) into IP addresses — the internet's phone book. **Public zones** resolve on the whole internet; **private zones** resolve only inside your VPC. Common records: **A** (name→IPv4), **CNAME** (alias), **MX** (mail), **TXT** (verification).

**Q25. What is Cloud CDN?**
A Content Delivery Network that caches content at Google's **edge locations** worldwide so users fetch it from a nearby spot instead of the origin — speeding up images/videos/static files and reducing backend load. It's enabled at the load balancer backend.

**Q26. VPC Peering vs Shared VPC?**
**VPC Peering** connects two VPCs so their VMs talk over internal IPs privately (a private bridge between two cities). **Shared VPC** has a **host project** own the VPC while **service projects** attach and use its subnets (a mall where many shops share the infrastructure) — great for central network/security teams.

**Q27. Cloud VPN vs Cloud Interconnect?**
**Cloud VPN** is an encrypted tunnel over the **public internet** (cheaper, quick — an armored courier on public roads). **Cloud Interconnect** is a **dedicated private physical link** to Google (expensive, but high bandwidth + low latency + private — your own highway). Direct/Partner Interconnect connects directly or via a partner.

**Q28. What is Private Google Access?**
It lets VMs **with no external IP** reach **Google APIs/services** (Cloud Storage, BigQuery, etc.) **without going out to the public internet** — a private staff corridor to the Google services counter. Related: **Private Service Connect**.

---

## Scenario / Cross-Topic Questions

**Q29. You launched a web server VM but can't reach it from your browser. What do you check?**
(1) Does the VM have an **external IP**? (2) Is there a **firewall allow rule** for the web port (TCP:80/443) targeting the VM (e.g., via a network tag)? (3) Is the web server actually running/listening? (4) Are DNS records pointing to the right IP? Remember ingress is blocked by default.

**Q30. You want VMs to download OS updates but not be exposed to the internet. How?**
Keep them with **internal IPs only** and configure **Cloud NAT** for outbound access. For reaching Google services specifically, also enable **Private Google Access**.

**Q31. Design a highly available public web app that survives a zone (and ideally region) outage.**
Use a **regional MIG** across multiple zones behind a **Global External HTTP(S) Load Balancer** (single anycast IP) with **health checks** and **autoscaling**, add **Cloud CDN** for static content, and **Cloud DNS** to map your domain. For region-level DR, run backends in multiple regions — the global LB routes users to the nearest healthy region.

**Q32. How would you let only your office IP SSH into your VMs?**
Create a firewall **allow rule** for `tcp:22` with `--source-ranges=<your-office-IP>/32` targeting the VMs (via network tag), instead of using `0.0.0.0/0`.

**Q33. Two teams in separate projects need their VMs to talk privately. What are the options?**
Use **VPC Peering** between their VPCs, or adopt a **Shared VPC** where a host project provides the network and both teams' projects attach as service projects.

---

## Quick `gcloud` Commands to Remember

```bash
# VPC + subnet + VM
gcloud compute networks create my-vpc --subnet-mode=custom
gcloud compute networks subnets create my-subnet \
  --network=my-vpc --region=asia-south1 --range=10.0.1.0/24
gcloud compute instances create app-vm \
  --zone=asia-south1-a --network=my-vpc --subnet=my-subnet

# Firewall rules
gcloud compute firewall-rules create allow-http \
  --network=my-vpc --direction=INGRESS --action=ALLOW \
  --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=web-server
gcloud compute firewall-rules create allow-ssh \
  --network=my-vpc --action=ALLOW --rules=tcp:22 --source-ranges=0.0.0.0/0

# Routes
gcloud compute routes list

# Static IP
gcloud compute addresses create my-static-ip --region=asia-south1
gcloud compute addresses list

# Cloud NAT (router + NAT)
gcloud compute routers create my-router --network=my-vpc --region=asia-south1
gcloud compute routers nats create my-nat \
  --router=my-router --region=asia-south1 \
  --auto-allocate-nat-external-ips --nat-all-subnet-ip-ranges

# Load balancer (health check + backend + url-map + proxy + forwarding rule)
gcloud compute health-checks create http my-hc --port=80
gcloud compute backend-services create my-backend \
  --protocol=HTTP --health-checks=my-hc --global
gcloud compute backend-services add-backend my-backend \
  --instance-group=web-mig --instance-group-region=asia-south1 --global
gcloud compute url-maps create my-map --default-service=my-backend
gcloud compute target-http-proxies create my-proxy --url-map=my-map
gcloud compute forwarding-rules create my-rule \
  --global --target-http-proxy=my-proxy --ports=80

# Cloud DNS
gcloud dns managed-zones create myzone \
  --dns-name="myshop.com." --description="public zone"
```
