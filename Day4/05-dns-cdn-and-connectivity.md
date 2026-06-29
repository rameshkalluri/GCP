# DNS, CDN & Connectivity

Rounding out Day 4: how users **find** your service (DNS), how to make it **fast** (CDN),
how to **connect networks** (peering, Shared VPC, VPN/Interconnect), and how private VMs
reach Google services (Private Google Access).

---

## 1. Cloud DNS — the internet's phone book 📖

**DNS (Domain Name System)** translates **names** (`www.myshop.com`) into **IP addresses**
(`34.120.45.10`). **Cloud DNS** is Google's managed DNS service.

> **Analogy:** DNS is the **phone book / contacts app**. You remember the **name**
> ("Mom"), and it looks up the actual **number** (IP) for you. Nobody memorizes IPs,
> just like nobody memorizes phone numbers anymore.

- **Public DNS zone** — names the whole internet can resolve (your website).
- **Private DNS zone** — names that only resolve **inside your VPC** (internal services).
- Common records: **A** (name→IPv4), **CNAME** (alias), **MX** (mail), **TXT** (verification).

```bash
gcloud dns managed-zones create myzone \
  --dns-name="myshop.com." --description="public zone"
```

---

## 2. Cloud CDN — keep content close to users ⚡

**Cloud CDN (Content Delivery Network)** caches your content at Google's **edge
locations** worldwide so users get it from a **nearby** spot instead of the origin server.

> **Analogy:** Instead of everyone driving to **one central warehouse**, you stock
> popular items in **local corner shops** in every city. Customers grab them next door —
> much faster.

- Speeds up images, videos, static files, and cacheable pages.
- Reduces load on your backend (fewer trips to origin).
- Turned on at the **load balancer** (Day 4) backend.

---

## 3. Connecting networks

### VPC Peering — a private bridge between two VPCs
- Connects **two VPCs** so their VMs talk over **internal IPs**, privately.
- *Analogy:* building a **private bridge** between two gated cities — residents visit
  directly without going out onto the public highway (internet).

### Shared VPC — one network shared by many projects
- A **host project** owns the VPC; **service projects** attach and use its subnets.
- *Analogy:* a **shopping mall**: the mall owner (host) provides the building and roads;
  individual shops (service projects) just move in and use the shared infrastructure.
- Great for central network/security teams to control networking org-wide.

### Hybrid connectivity (cloud ↔ on-premises)
| Option | What it is | Analogy |
|---|---|---|
| **Cloud VPN** | Encrypted tunnel over the **public internet** | A **secure armored courier** on public roads |
| **Cloud Interconnect** | A **dedicated private physical link** to Google | Your **own private highway** to the data center |
| **Direct/Partner Interconnect** | Connect via Google or a partner | Private highway built **directly** or **via a contractor** |

- VPN = cheaper, quick, good enough for many cases (but rides public internet).
- Interconnect = expensive, but **high bandwidth + low latency + private**.

---

## 4. Private Google Access (PGA) 🔒
Lets VMs **with no external IP** reach **Google APIs/services** (Cloud Storage, BigQuery,
etc.) **without going out to the public internet**.

> **Analogy:** A **private staff corridor** from your office straight to the Google
> services counter — you never step outside onto the public street.

- Pairs well with keeping VMs private (security) while still using Google services.
- Related: **Private Service Connect** for privately reaching services/APIs.

---

## How it fits with earlier Day 4 notes
```
DNS         → users find your front door by name
Load Balancer + CDN → spread traffic + serve cached content fast
VPC Peering / Shared VPC → connect private networks together
VPN / Interconnect → connect cloud to your on-prem datacenter
Private Google Access → private VMs reach Google services without the internet
```

---

## TL;DR (in plain English)
- **Cloud DNS = the phone book**: turns names into IPs (public or private zones).
- **Cloud CDN = local corner shops**: cache content at the edge for speed.
- **VPC Peering = a private bridge** between two VPCs; **Shared VPC = a mall** where many
  projects use one central network.
- **Cloud VPN = armored courier on public roads**; **Interconnect = your own private
  highway** to Google (hybrid/on-prem).
- **Private Google Access = a staff corridor** letting private VMs reach Google services
  without touching the internet.
