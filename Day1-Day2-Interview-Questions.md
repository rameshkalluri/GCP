# GCP Day 1 + Day 2 — Interview Questions & Answers

A curated set of interview questions with answers covering **Day 1** (Cloud
Fundamentals, Resource Hierarchy, Billing, Console/Cloud Shell, APIs) and **Day 2**
(IAM Deep Dive, Service Accounts, Least Privilege, Audit Logs & Org Policy).

> Note: `Day1/03-regions-and-zones.md` already has a dedicated regions/zones Q&A
> section — this file focuses on the remaining topics plus cross-topic scenarios.

---

## Day 1 — Cloud Fundamentals, Hierarchy, Billing, Console

### Fundamentals (IaaS/PaaS/SaaS)

**Q1. Explain IaaS, PaaS, and SaaS with GCP examples.**
- **IaaS** — you manage OS/runtime/app/data; provider manages hardware/virtualization/network. Example: **Compute Engine, VPC, Persistent Disk**.
- **PaaS** — you manage app + data; provider handles OS/runtime/scaling. Example: **App Engine, Cloud Run, Cloud SQL**.
- **SaaS** — just use it; provider manages everything. Example: **Google Workspace, Gmail**.
- **FaaS** (serverless) — you supply only function code. Example: **Cloud Functions**.
- Rule of thumb: IaaS → PaaS → SaaS means **less control but less ops burden**.

**Q2. What is the difference between public, private, hybrid, and multi-cloud?**
Public = provider-owned (GCP/AWS/Azure); private = dedicated to one org; hybrid = mix of on-prem + cloud (e.g., GCP **Anthos**); multi-cloud = using more than one provider.

**Q3. What are the ways to interact with GCP?**
Cloud Console (web UI), gcloud CLI/Cloud SDK, Cloud Shell, client libraries (Python/Java/Go/etc.), and REST/gRPC APIs.

### Resource Hierarchy

**Q4. Describe the GCP resource hierarchy.**
**Organization → Folder → Project → Resources.** Organization is the company root (one per domain, tied to Cloud Identity/Workspace). Folders group projects/folders (by dept/team/env) and allow delegated admin. Projects are the fundamental unit — required to use any service, and they act as a **billing + security boundary**. Resources (VMs, buckets, etc.) live inside exactly one project.

**Q5. What are the three identifiers of a project?**
- **Project ID** — globally unique, user-chosen, **immutable**.
- **Project Name** — human-friendly, editable, not unique.
- **Project Number** — auto-generated, globally unique.

**Q6. How does policy inheritance work? What does "IAM is additive" mean?**
Policies set at a higher level **flow down** to everything beneath. Effective policy = **union** of policies from the resource up to the org. "Additive" means a child **cannot remove** an inherited allow — to restrict, you use **Org Policy constraints, IAM Conditions, or Deny policies**.

**Q7. IAM Policy vs Organization Policy?**
IAM answers **"who can do what?"** (members + roles). Org Policy answers **"what is allowed to exist/be configured?"** (constraints like "only allow VMs in us-central1"). Even an Owner can't break an Org Policy constraint.

### Billing

**Q8. How is GCP billing structured?**
**Billing Account → Project(s) → Resources.** A billing account holds payment info; one account can fund many projects; a project must be linked to a billing account to use paid services.

**Q9. Do budget alerts stop your spending?**
**No** — budgets only **notify** at thresholds (50/90/100%). To enforce a hard stop, wire the budget alert to **Pub/Sub → Cloud Function** that disables billing or shuts down resources.

**Q10. Name ways to reduce GCP cost.**
Sustained use discounts, Committed Use Discounts (1–3 yr), Spot/Preemptible VMs (~60–91% cheaper), right-sizing, autoscaling/scale-to-zero, storage lifecycle (Nearline/Coldline/Archive), and deleting idle resources.

### Console / Cloud Shell / APIs

**Q11. What is Cloud Shell and what makes it convenient?**
A free browser-based Linux VM with the SDK pre-installed (gcloud, gsutil, bq, kubectl, Docker), **already authenticated** as your console user, with a **5 GB persistent home** directory. The VM is ephemeral but `$HOME` survives.

**Q12. Why do you sometimes get an "API not enabled" error?**
Almost every GCP service is **off until you enable its API** for the project (e.g., `compute.googleapis.com`). Two separate gates must both be open: **API enabled** (service switched on) AND **IAM permission** (you're allowed to use it).

**Q13. What are quotas?**
Usage caps on APIs — **rate quotas** (calls/minute) and **allocation quotas** (e.g., CPUs per region). They protect against runaway cost; you can request increases.

---

## Day 2 — IAM Deep Dive, Service Accounts, Least Privilege, Audit/Org Policy

### IAM Deep Dive

**Q14. State the core IAM model in one line.**
**WHO (member) + WHAT (role) + WHICH (resource) = permission to act.** A policy binds members to roles on a resource.

**Q15. What are the types of members/principals?**
Google Account (user), Service Account (app/robot), Google Group, Domain (Workspace/Cloud Identity), and the special **allUsers / allAuthenticatedUsers** (public — use with care).

**Q16. Explain the three role types and when to use each.**
- **Primitive/Basic** (Owner, Editor, Viewer) — broad, legacy; **avoid in prod**.
- **Predefined** (e.g., `roles/storage.objectViewer`) — curated per service; **preferred default**.
- **Custom** — you choose exact permissions for true least privilege.

**Q17. What does a permission look like, and do you grant permissions directly?**
Format is `service.resource.verb`, e.g. `compute.instances.start`. You **don't** grant permissions directly — you grant **roles**, which bundle permissions.

### Service Accounts

**Q18. What is a service account and why is it "both an identity and a resource"?**
A non-human identity for apps/VMs/services. It's an **identity** (you grant it roles → what it can do) **and** a **resource** (you control who can "act as"/impersonate it).

**Q19. Why are service account keys risky, and what are better alternatives?**
Downloaded JSON keys are like photocopied house keys — they don't expire on their own and leaked keys are a top cloud incident. Prefer **attached service accounts** (VM/Cloud Run simply runs as the SA) and **Workload Identity Federation** (external workloads borrow access, no key file). If you must use a key: store in **Secret Manager**, rotate, delete unused, never commit to Git.

**Q20. Service account best practices?**
One SA **per workload** with least privilege; prefer Workload Identity over keys; rotate/delete keys; don't use the broad **default** SA for real workloads; never store keys in code/Git.

### Least Privilege & Security

**Q21. What is least privilege and why does it matter?**
Give each person/app **only the access needed, for the shortest time**. Benefits: smaller blast radius if compromised, fewer accidental mistakes, easier audits.

**Q22. Describe defense-in-depth layers in GCP.**
Identity (MFA) → IAM roles (least privilege) → resource boundaries (separate dev/prod projects) → Org Policies (guardrails) → network (firewalls, private IPs) → data (encryption + Secret Manager) → monitoring (audit logs).

**Q23. What are IAM Conditions?**
Context/time-based access — e.g., a role that works **only until a date** (contractor) or **only on resources with a certain name/tag**. Like a key card that only works 9am–5pm or only on the 3rd floor.

### Audit Logs & Org Policy

**Q24. What are the four types of Cloud Audit Logs and which are on by default?**
- **Admin Activity** — config/resource changes (create/delete VM, IAM changes). **Always on, free.**
- **Data Access** — who read/wrote data. **Off by default** (noisy/costly).
- **System Event** — Google-initiated actions. **On.**
- **Policy Denied** — access blocked by policy. **On.**

**Q25. How do IAM, Org Policy, and Audit Logs work together?**
**Org Policy** = set guardrails up front (what can exist/be configured). **IAM** = control who can act within those guardrails. **Audit Logs** = the record to verify/investigate afterward. Analogy: house rules + key cards + CCTV.

**Q26. Give examples of common Org Policy constraints.**
Restrict resource locations (data residency), disable service account key creation, skip default network creation, restrict public IPs on VMs, restrict external sharing/domains.

---

## Scenario / Cross-Topic Questions

**Q27. A developer says "my code can't access the bucket" even though they're an Owner. What do you check?**
Two gates: (1) Is the **API enabled** (`storage.googleapis.com`)? (2) Is the code running as the right identity (the **service account**, not the human user) and does that SA have the right role? Also check Org Policy / Deny policies and IAM Conditions that could restrict access despite the Owner role.

**Q28. How would you enforce "VMs may only be created in asia-south1" company-wide?**
Use an **Organization Policy** constraint restricting resource locations — not IAM. Even Owners can't override it. IAM still controls *who* can create VMs.

**Q29. How do you give a whole team access without managing users one by one?**
Grant roles to a **Google Group** (e.g., `developers@company.com`) rather than individuals — easier to manage as people join/leave.

**Q30. A contractor needs temporary access until end of month. How?**
Grant the role with an **IAM Condition** that expires on a specific date, scoped to the lowest level (project/resource) that works.

---

## Quick `gcloud` Commands to Remember

```bash
# Projects & hierarchy
gcloud projects list
gcloud projects get-iam-policy PROJECT_ID

# Grant / revoke a role
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:alice@example.com" --role="roles/viewer"
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member="user:alice@example.com" --role="roles/viewer"

# Service accounts
gcloud iam service-accounts create my-sa --display-name="My App SA"
gcloud iam service-accounts list

# APIs / services
gcloud services list --enabled
gcloud services enable compute.googleapis.com

# Billing
gcloud billing accounts list
gcloud billing projects link PROJECT_ID --billing-account=BILLING_ACCOUNT_ID

# Audit logs & org policy
gcloud logging read "logName:cloudaudit.googleapis.com%2Factivity" --limit=10
gcloud resource-manager org-policies list --project=PROJECT_ID
```
