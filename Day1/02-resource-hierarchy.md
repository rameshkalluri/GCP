# GCP Resource Hierarchy

> **Organization → Folder → Project → Resources**
> A top-down tree that lets you organize, manage, and control access to everything in Google Cloud.

---

## Why it matters
- **Organize** resources logically (by department, team, environment, app).
- **Apply IAM policies** at any level — they **inherit downward**.
- **Apply Org Policies** (guardrails/constraints) centrally.
- **Track billing** and group costs.

---

## The 4 Levels

```
Organization (your company: example.com)
│
├── Folder (e.g., Department: Engineering)
│     │
│     ├── Folder (e.g., Team: Payments)
│     │     ├── Project (payments-dev)
│     │     ├── Project (payments-prod)
│     │     └── ...
│     │
│     └── Project (shared-tools)
│
└── Folder (e.g., Department: Marketing)
      └── Project (marketing-web)
            └── Resources (VMs, buckets, SQL, GKE, ...)
```

### 1. Organization (root node)
- The **top** of the hierarchy; represents your company.
- Created automatically when you have **Google Workspace** or **Cloud Identity**.
- Tied to a **domain** (e.g., `example.com`).
- Special roles: **Organization Admin**, **Org Policy Admin**.
- There is **one organization per domain**.

### 2. Folders (optional, but recommended)
- Group projects (and other folders) — e.g., by department, team, or environment.
- Allow **delegated administration** (give a team admin rights to their folder only).
- Can be **nested** (folders within folders).
- **Limit:** up to ~10 levels deep; require an Organization to exist.

### 3. Projects (the core unit)
- The **fundamental** unit — almost everything lives in a project.
- Required to **use any GCP service**, create resources, enable APIs, and bill usage.
- Each project has three identifiers:
  - **Project ID** — globally unique, user-chosen, *immutable* (e.g., `my-app-prod-1234`).
  - **Project Name** — human-friendly, editable, not unique.
  - **Project Number** — auto-generated, globally unique.
- Acts as a **trust/security boundary** and a **billing boundary**.

### 4. Resources (the leaves)
- The actual services you create **inside a project**:
  - Compute Engine VMs, Cloud Storage buckets, Cloud SQL, GKE clusters,
    Cloud Functions, Pub/Sub topics, BigQuery datasets, VPC networks, etc.
- Each resource belongs to **exactly one project**.

---

## Policy Inheritance (the key concept)

IAM and Organization policies set at a higher level **flow down** to everything beneath it.

```
Org policy        →  applies to ALL folders, projects, resources
Folder policy     →  applies to projects/resources in that folder
Project policy    →  applies to resources in that project
```

- A child **inherits** the parent's policies and can **add** its own.
- The **effective policy** = union of policies from the resource up to the org.
- **IAM is additive** — you cannot remove an inherited *allow* at a lower level
  (use **Org Policy constraints / IAM Conditions / Deny policies** to restrict).

**Example:** Grant `roles/viewer` to a user at the Organization → that user can
view resources in **every** project under the org.

---

## IAM vs Org Policy (don't confuse them)

| | **IAM Policy** | **Organization Policy** |
|---|---|---|
| Question it answers | **Who** can do **what**? | **What** is **allowed to exist/configure**? |
| Example | "Alice can manage VMs" | "Only allow VMs in `us-central1`" |
| Scope | Members + roles | Constraints on resource config |
| Inherited? | Yes (additive) | Yes (can be enforced/merged) |

---

## Common Hierarchy Patterns
- **By environment:** Folders `dev`, `staging`, `prod` → projects per app.
- **By team/department:** Folders per team → projects per service.
- **Hybrid:** `Org → Dept folder → Env folder → Project`.
- Keep **prod** isolated in its own folder/project with tighter IAM.

---

## Best Practices
- Use **folders** to mirror your org structure and delegate admin.
- **One app + environment = one project** (clean isolation & billing).
- Apply **least-privilege IAM** at the **lowest** level that works.
- Set **Org Policy constraints** early (regions, allowed services, etc.).
- Use a **dedicated project** for shared/common resources.
- Standardize **naming conventions** and **labels** for cost tracking.

---

## Quick `gcloud` Reference

```bash
# Organizations
gcloud organizations list

# Folders
gcloud resource-manager folders list --organization=ORG_ID
gcloud resource-manager folders create --display-name="Engineering" --organization=ORG_ID

# Projects
gcloud projects list
gcloud projects create my-app-prod-1234 --folder=FOLDER_ID
gcloud projects describe my-app-prod-1234

# View effective IAM policy at each level
gcloud organizations get-iam-policy ORG_ID
gcloud resource-manager folders get-iam-policy FOLDER_ID
gcloud projects get-iam-policy PROJECT_ID
```

---

## TL;DR
- **Organization** = company root (one per domain).
- **Folders** = optional grouping (dept/team/env), can nest, enable delegation.
- **Projects** = where resources live, billing + security boundary.
- **Resources** = the actual services (VMs, buckets, DBs...).
- **Policies inherit downward** and IAM is **additive** — design top-down with least privilege.
