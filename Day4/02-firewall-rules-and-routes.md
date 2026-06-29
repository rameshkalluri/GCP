# Firewall Rules & Routes

Two things control how traffic moves: **firewall rules** (who's allowed in/out) and
**routes** (which way the traffic travels).

---

## Firewall Rules = Security Gates 🚧

A **firewall rule** decides whether network traffic is **allowed** or **denied**.

> **Analogy:** Firewall rules are the **security guards at the city gates**. They check
> each car: "Are you allowed through this gate? Where are you coming from? Which door
> are you trying to reach?" — and then say **yes (allow)** or **no (deny)**.

### Every firewall rule has:
| Part | Meaning | Analogy |
|---|---|---|
| **Direction** | Ingress (incoming) or Egress (outgoing) | Cars coming **in** vs going **out** |
| **Action** | Allow or Deny | Guard says **yes** or **no** |
| **Target** | Which VMs it applies to (via tags/SA) | Which **buildings** the rule protects |
| **Source/Destination** | Which IPs are affected | Where the car is **from / going to** |
| **Protocol & Port** | e.g. TCP:80 (web), TCP:22 (SSH) | Which **door** (door 80 = web, door 22 = SSH) |
| **Priority** | Lower number = checked first (wins) | Senior guard's order **overrides** juniors |

### Key facts (the defaults)
- By default: **all incoming traffic is blocked**, **all outgoing is allowed**.
  - *Analogy:* gates are **shut to visitors** by default, but your people can **leave** freely.
- You open specific doors by **adding allow rules** (e.g., allow web on port 80).
- **Priority**: rules are checked from **lowest number (0) to highest (65535)**; the
  first match wins.
  - *Analogy:* the **most senior guard** (lowest number) gives the final order.

### Network Tags (apply rules to groups of VMs)
- A **tag** is a label on a VM (e.g., `web-server`).
- Firewall rules can target tags: "allow port 80 to all VMs tagged `web-server`."
- *Analogy:* a **uniform/badge color** — "let visitors into all buildings with a
  **blue badge**."

```bash
# Allow web traffic (port 80) to VMs tagged "web-server"
gcloud compute firewall-rules create allow-http \
  --network=my-vpc --direction=INGRESS --action=ALLOW \
  --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=web-server

# Allow SSH (port 22) so you can log in
gcloud compute firewall-rules create allow-ssh \
  --network=my-vpc --action=ALLOW --rules=tcp:22 --source-ranges=0.0.0.0/0
```

> ⚠️ `0.0.0.0/0` means **"from anywhere on the internet."** Fine for a public web port
> (80/443); for SSH, prefer limiting to your own IP for safety.

---

## Routes = Road Signs / Maps 🗺️

A **route** tells traffic **which path to take** to reach a destination.

> **Analogy:** Routes are the **road signs and GPS directions**. "To reach the
> internet, take this highway. To reach the database neighborhood, turn here."

### Key facts
- GCP creates **default routes** automatically:
  - A route so VMs can reach **each other** within the VPC (local roads).
  - A **default internet route** (`0.0.0.0/0`) to reach the outside world.
- You can add **custom routes** to send traffic through a specific device
  (e.g., a firewall appliance or VPN).
  - *Analogy:* adding a **detour sign** so cars pass through a **checkpoint** first.

```bash
# See the routes (road map) for your project
gcloud compute routes list
```

---

## Firewall vs Route — don't confuse them
| | **Firewall Rule** | **Route** |
|---|---|---|
| Question | **Is this traffic allowed?** | **Which way does it go?** |
| Analogy | Security **gate** (yes/no) | **Road sign / GPS** (direction) |
| Example | "Allow port 80 from internet" | "Send internet-bound traffic to the gateway" |

> Both must cooperate: a route gets the car **to the gate**, and the firewall decides
> if it can **pass through**.

---

## TL;DR (in plain English)
- **Firewall rules = security gates**: allow/deny traffic by direction, source, port, tag.
- Defaults: **incoming blocked, outgoing allowed** — you open doors with allow rules.
- **Priority**: lowest number wins; use **network tags** to target groups of VMs.
- **Routes = road signs**: they decide the **path** traffic takes (e.g., to the internet).
- Firewall = *"allowed?"*; Route = *"which way?"* — both work together.
