# IAM Basics (Identity and Access Management)

IAM answers one question: **WHO can do WHAT on WHICH resource?**

```
Member (who)  +  Role (what)  →  bound on a Resource (where)  =  IAM Policy
```

---

## The 3 Core Pieces

### 1. Members (the "who" / principals)
- **Google Account** — an individual user (`user:alice@example.com`).
- **Service Account** — an identity for apps/VMs (`...@project.iam.gserviceaccount.com`).
- **Google Group** — a collection of users (`group:devs@example.com`).
- **Google Workspace / Cloud Identity domain** — all users in a domain.
- **allAuthenticatedUsers / allUsers** — special public identifiers (use with care).

### 2. Roles (the "what" — a bundle of permissions)
- **Primitive (basic) roles** — broad, legacy: `Owner`, `Editor`, `Viewer`.
  - Avoid in production (too coarse-grained).
- **Predefined roles** — curated per service, e.g. `roles/compute.instanceAdmin`,
  `roles/storage.objectViewer`. **Preferred** default.
- **Custom roles** — you pick the exact permissions (least privilege).

> A **permission** looks like `service.resource.verb`, e.g. `compute.instances.start`.
> You don't grant permissions directly — you grant **roles**, which contain permissions.

### 3. Policy (the binding)
A policy binds **members → roles** on a resource. Attached at any level of the
hierarchy (org/folder/project/resource).

---

## Policy Inheritance
- Policies set higher in the hierarchy **inherit downward**
  (Org → Folder → Project → Resource).
- **IAM is additive (allow-only)** — a child cannot remove an inherited allow.
- Effective access = **union** of all inherited policies.
- To restrict, use **IAM Conditions**, **Org Policy constraints**, or **Deny policies**.

---

## Service Accounts (important!)
- A **non-human identity** used by applications, VMs, and services.
- Can be **granted roles** (what *it* can do) and can have **users granted access to it**
  (who can *act as* it).
- Authenticate via **keys** (avoid long-lived keys) or, better, **attached identities**
  / Workload Identity.
- Best practices:
  - One service account **per workload/app** with least privilege.
  - Prefer **Workload Identity** over downloaded JSON keys.
  - Rotate/disable unused keys; audit usage.

---

## Least Privilege — Best Practices
- Grant the **minimum role** needed, at the **lowest level** that works.
- Prefer **predefined** over primitive roles; use **custom** when needed.
- Assign roles to **groups**, not individuals, for easier management.
- Use **IAM Conditions** (time-bound, resource-based) for fine control.
- Regularly review with the **IAM Recommender** and audit logs.

---

## Quick `gcloud` Reference

```bash
# View a project's IAM policy
gcloud projects get-iam-policy PROJECT_ID

# Grant a role to a user
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:alice@example.com" --role="roles/viewer"

# Revoke a role
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member="user:alice@example.com" --role="roles/viewer"

# Create a service account
gcloud iam service-accounts create my-sa --display-name="My App SA"

# Grant a role to a service account
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:my-sa@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/storage.objectViewer"

# List predefined roles / inspect one
gcloud iam roles list
gcloud iam roles describe roles/compute.instanceAdmin
```

---

## TL;DR
- IAM = **who (member)** can do **what (role)** on **which resource**.
- Roles: **primitive** (avoid) → **predefined** (default) → **custom** (least privilege).
- Policies **inherit downward** and IAM is **additive**.
- **Service accounts** = identities for apps/VMs; prefer Workload Identity over keys.
- Always apply **least privilege**.
