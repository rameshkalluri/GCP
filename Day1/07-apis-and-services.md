# APIs & Services

## Why this matters
In GCP, **almost every service is "off" until you switch it on** for your project by
**enabling its API**. New learners often hit an error like *"API not enabled"* — this
note explains why.

> **Analogy:** A new project is like a **brand-new phone with no apps installed**. The
> hardware is there, but before you can use Maps or WhatsApp, you have to **install
> (enable) each app**. In GCP, "installing the app" = **enabling the API** for that service.

---

## What is an API here?
Every GCP service (Compute, Storage, Cloud SQL, Vertex AI...) exposes an **API** —
the doorway through which the Console, gcloud, and your code all talk to it.

- Use Compute Engine → enable **`compute.googleapis.com`**.
- Use Cloud Storage → enable **`storage.googleapis.com`**.
- Use Cloud SQL → enable **`sqladmin.googleapis.com`**.

> **Analogy:** Each API is the **service counter** for that department. If the counter
> is closed (API disabled), you can't be served — even if you have permission.

---

## Enable vs Permission (don't confuse them)
Two separate gates must both be open:
1. **API enabled?** → is the service *switched on* for this project? (this note)
2. **IAM permission?** → are *you allowed* to use it? (Day 1/2 IAM)

> **Analogy:** The **shop must be open** (API enabled) **and** you must have a **valid
> membership card** (IAM role). Both are needed to buy anything.

---

## APIs & Services in the Console
Find it under **Navigation menu → APIs & Services**:
- **Library** — browse and enable APIs (the "app store").
- **Enabled APIs & Services** — what's already on + usage dashboards.
- **Credentials** — API keys, OAuth clients, service account access.
- **Quotas** — limits on how much you can call an API (see below).

---

## Quotas & Limits (guardrails)
APIs have **quotas** — caps on usage (e.g., requests/minute, number of VMs/region).

> **Analogy:** Like a **fair-use limit** at a buffet — you can take a lot, but not
> empty the whole kitchen at once. Protects you (runaway costs) and the platform.

- **Rate quotas** — how many calls per minute/day.
- **Allocation quotas** — how many resources you can have (e.g., CPUs per region).
- You can **request increases** when you genuinely need more.

---

## Quick `gcloud` Reference
```bash
# List services you can enable
gcloud services list --available

# See what's enabled in your project
gcloud services list --enabled

# Enable / disable a service (install/uninstall the "app")
gcloud services enable compute.googleapis.com
gcloud services disable compute.googleapis.com
```

---

## TL;DR (in plain English)
- A new project has services **switched off** — you **enable each API** to use it
  (like installing apps on a new phone).
- Two gates: **API enabled** (shop open) **and** **IAM permission** (membership card).
- Manage under **APIs & Services** (Library / Enabled / Credentials / Quotas).
- **Quotas** are fair-use caps you can raise on request.
