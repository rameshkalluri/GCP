# IAM Deep Dive (Identity & Access Management)

## The One Sentence to Remember
**IAM decides WHO can do WHAT on WHICH thing.**

```
WHO (member)  +  WHAT (role)  +  WHICH (resource)  =  permission to act
```

---

## The Hotel Analogy

Imagine GCP is a **hotel**:

| Hotel thing | GCP thing | Meaning |
|---|---|---|
| A **guest / staff member** | **Member (principal)** | The *who* — a person or an app |
| A **key card** | **Role** | What doors/actions it unlocks |
| A **room or floor** | **Resource** | The *which* — the thing being accessed |
| The **front-desk rulebook** | **IAM Policy** | "This card opens these rooms" |

A cleaner gets a key card that opens **guest rooms** (to clean them) but **not** the
**manager's office** or the **safe**. That's IAM: the right access, nothing more.

---

## 1. Members (the WHO)
These are the identities that can be given access:

- **Google Account** → a real person (`user:alice@gmail.com`).
  - *Analogy:* a hotel guest with their own name badge.
- **Service Account** → a robot/app identity (`my-app@project.iam.gserviceaccount.com`).
  - *Analogy:* a cleaning robot that has its own key card (no human holds it).
- **Google Group** → a bunch of people together (`group:developers@company.com`).
  - *Analogy:* "all housekeeping staff" — give the whole team access at once.
- **Domain** → everyone in your company.
- **allUsers / allAuthenticatedUsers** → the public / anyone logged in.
  - *Analogy:* leaving a door **unlocked for everyone** — use very carefully!

---

## 2. Roles (the WHAT)
A role is a **bundle of permissions** — a key card programmed to open certain doors.

There are **3 types**:

### a) Primitive (Basic) roles — the "master keys"
- `Owner`, `Editor`, `Viewer`.
- Very broad and powerful.
- *Analogy:* a **master key** that opens almost everything.
- 👉 Avoid in real projects — too much access.

### b) Predefined roles — the "purpose-made keys" ✅ (use these)
- Built by Google for specific jobs, e.g.:
  - `roles/storage.objectViewer` → can only **read** files in Cloud Storage.
  - `roles/compute.instanceAdmin` → can manage VMs.
- *Analogy:* a key card labeled **"Housekeeping — guest rooms only."**

### c) Custom roles — the "tailor-made keys"
- You pick the exact permissions yourself.
- *Analogy:* a key card you program door-by-door for a special job.
- Use when predefined roles don't fit perfectly.

> A single **permission** looks like `service.resource.verb`
> e.g. `compute.instances.start` = "allowed to start a VM."
> You don't hand out permissions one by one — you hand out **roles** (key cards).

---

## 3. Policy (the rulebook that ties it together)
An **IAM Policy** is the list of bindings: *"these members get this role on this resource."*

```
Policy on Project "shop-prod":
  - alice@company.com        → Viewer
  - developers@company.com   → Compute Admin
  - billing-bot (SA)         → Billing Viewer
```

*Analogy:* the **front-desk rulebook** saying which badge opens which rooms.

---

## Inheritance — access flows downhill 🏔️

Remember the hierarchy from Day 1: **Organization → Folder → Project → Resource**.

Access given at the top **flows down** to everything below it.

- *Analogy:* If you get a key card for the **whole building**, it also opens every
  **floor** and **room** inside. You can't be given building-wide access and then be
  "blocked" from one room just by a lower rule — IAM only **adds** access.

Key facts:
- **IAM is additive** (allow-only). Lower levels can **add** access, not subtract it.
- To actually **restrict**, use **Org Policies**, **IAM Conditions**, or **Deny policies**.

---

## Quick `gcloud` Reference

```bash
# See who has access to a project
gcloud projects get-iam-policy PROJECT_ID

# Give Alice "Viewer" on the project
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:alice@company.com" --role="roles/viewer"

# Take it away
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member="user:alice@company.com" --role="roles/viewer"

# Explore available roles
gcloud iam roles list
gcloud iam roles describe roles/storage.objectViewer
```

---

## TL;DR (in plain English)
- IAM = **who** gets **which key card (role)** for **which room (resource)**.
- **Members:** people, apps (service accounts), groups, or the public.
- **Roles:** primitive (master key — avoid), predefined (purpose-made — use), custom (tailor-made).
- **Policy:** the rulebook tying members to roles on a resource.
- Access **flows downhill** and is **additive** — give the *least* needed.
