# VPC & Subnets

## What is a VPC?
A **VPC (Virtual Private Cloud)** is your own **private network** inside Google Cloud,
where your VMs and services live and talk to each other.

> **Analogy:** A VPC is like a **gated private city** that belongs only to you. Inside
> it, your buildings (VMs) can talk to each other over **private roads**. The outside
> world can't just walk in — there are gates (firewalls) controlling entry.

### Key fact: GCP VPC is GLOBAL 🌍
- One VPC can span **every region in the world**.
- *Analogy:* your private road system can stretch across **many cities (regions)** but
  still be **one connected network**.
- (This is different from AWS, where a VPC lives in just one region.)

---

## What is a Subnet?
A **subnet** is a slice of the VPC's address space that lives in **one region**.

> **Analogy:** If the VPC is the whole **city**, a subnet is a **neighborhood** within
> it. Each neighborhood has its own **range of street addresses** (IP range).

### Key facts
- **Subnets are regional** — a subnet covers **all zones** in its region.
  - *Analogy:* a neighborhood spans the whole **district**, not just one block.
- Each subnet has an **IP range** (CIDR), e.g. `10.0.1.0/24` = a block of addresses.
- VMs in a subnet get an **internal IP** from that range.

> **Remember (GCP vs AWS):** GCP **VPC = global**, **subnet = regional** (spans zones).
> AWS VPC = regional, subnet = one zone.

---

## CIDR / IP Ranges (the "address book")
A subnet's range looks like `10.0.1.0/24`.

> **Analogy:** Think of it as **"Maple Street, house numbers 0–255."** The `/24` just
> says how many house numbers are in that street.

Quick intuition:
- Smaller number after `/` = **bigger** range (more addresses).
  - `/16` ≈ 65,000 addresses (a huge area), `/24` ≈ 256 addresses (one street).
- Use **private ranges** (`10.x`, `172.16–31.x`, `192.168.x`) for internal IPs.

---

## Default vs Custom VPC
- **Default VPC** — auto-created with subnets in every region + basic firewall rules.
  - *Analogy:* a **pre-built starter town** — convenient for learning/testing.
- **Custom VPC** — you design subnets and ranges yourself.
  - *Analogy:* **town planning** done your way — recommended for real projects.
- **Auto mode** = subnets created automatically per region; **Custom mode** = you choose.

---

## How VMs Communicate
- VMs in the **same VPC** can talk over **internal IPs** (private roads) — even across
  regions — subject to firewall rules.
- To connect **two different VPCs**, use **VPC Peering** or a **Shared VPC**.
  - *Analogy:* building a **bridge** between two private cities.

---

## Quick `gcloud` Reference
```bash
# Create a custom-mode VPC (you'll add subnets yourself)
gcloud compute networks create my-vpc --subnet-mode=custom

# Add a subnet (neighborhood) in a region with an IP range
gcloud compute networks subnets create my-subnet \
  --network=my-vpc --region=asia-south1 --range=10.0.1.0/24

# List networks and subnets
gcloud compute networks list
gcloud compute networks subnets list

# Launch a VM into your subnet
gcloud compute instances create app-vm \
  --zone=asia-south1-a --network=my-vpc --subnet=my-subnet
```

---

## TL;DR (in plain English)
- **VPC = your private network** (a gated city). In GCP it's **global**.
- **Subnet = a neighborhood** in one **region** (spans all its zones) with an **IP range**.
- **CIDR** like `10.0.1.0/24` = a block of house addresses; smaller `/number` = more addresses.
- Use a **custom VPC** for real projects; connect VPCs with **peering / Shared VPC**.
