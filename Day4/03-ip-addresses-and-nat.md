# IP Addresses & Cloud NAT

## IP Addresses = House Addresses 🏠

An **IP address** is the unique address used to find and talk to a machine on a network.

> **Analogy:** Just like every house needs an **address** so mail can reach it, every
> VM needs an **IP address** so data can reach it.

---

## Internal vs External IPs

| Type | Reachable from | Analogy |
|---|---|---|
| **Internal (private) IP** | Only inside your VPC | Your **room number inside a building** — only people inside use it |
| **External (public) IP** | The whole internet | Your **full street address** the postman/world can find |

- Every VM gets an **internal IP** automatically (for private VPC chatter).
- An **external IP** is **optional** — only needed if the VM must be reached **from the
  internet** (e.g., a public web server).
- *Analogy:* most staff only need a **desk extension** (internal). Only the **front
  desk** needs a **public phone number** (external).

---

## Static vs Ephemeral IPs

| | **Ephemeral** | **Static** |
|---|---|---|
| Lifespan | Changes when VM stops/starts | Stays the same (reserved) |
| Analogy | A **temporary visitor pass** | A **permanent reserved address** |
| Use for | Test VMs, short-lived | Production servers, DNS records, whitelists |

- **Ephemeral** = assigned automatically, may change. Fine for temporary stuff.
- **Static** = you **reserve** it so it never changes (needed when others must always
  find you at the same address).

```bash
# Reserve a static external IP
gcloud compute addresses create my-static-ip --region=asia-south1

# List reserved addresses
gcloud compute addresses list
```

> 💡 A **reserved static IP that's NOT attached** to anything still **costs money** —
> release it if unused.

---

## Cloud NAT = Private Buildings Going Outside Safely 🚪➡️🌍

**Problem:** A VM with **only an internal IP** (no external IP) can't normally reach
the internet — but it might still need to **download updates or call an API**.

**Cloud NAT (Network Address Translation)** lets such **private VMs reach the internet
for outgoing requests**, *without* giving them a public address.

> **Analogy:** Imagine an **office building** where staff have no personal public phone
> number. They can still **call outside** through the building's **single switchboard**
> (NAT). Outsiders can't call *in* to them directly, but staff can call *out*.

### Why it's great for security
- VMs stay **hidden from the internet** (no inbound exposure).
- They can still **pull updates / call external APIs** (outbound only).
- *Analogy:* one-way glass — **you can see out, but no one can see in.**

```bash
# 1) A router is needed for NAT
gcloud compute routers create my-router \
  --network=my-vpc --region=asia-south1

# 2) Configure Cloud NAT on it
gcloud compute routers nats create my-nat \
  --router=my-router --region=asia-south1 \
  --auto-allocate-nat-external-ips \
  --nat-all-subnet-ip-ranges
```

---

## Quick Decision Guide
- VM only talks to other VMs privately → **internal IP only**.
- VM must be reached from the internet → add an **external IP** (static if it must be fixed).
- Private VM needs to reach **out** to the internet but stay hidden → **Cloud NAT**.

---

## TL;DR (in plain English)
- **IP address = a machine's house address.**
- **Internal IP** = private (inside the VPC); **External IP** = public (internet-facing).
- **Ephemeral** IPs can change; **static** IPs are reserved and fixed (release if unused — they cost money).
- **Cloud NAT** = a switchboard letting **private VMs call out** to the internet while
  staying **invisible to inbound** traffic (great for security).
