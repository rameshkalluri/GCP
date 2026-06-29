# Audit Logs & Organization Policy

Two important security tools: **Audit Logs** (the record of *what happened*) and
**Organization Policies** (company-wide *guardrails*).

---

## Part A — Cloud Audit Logs

**Audit Logs** record **who did what, where, and when** in your GCP environment.

> **Analogy:** Like the **CCTV cameras + visitor sign-in book** of your office building.
> If something goes wrong, you can rewind and see exactly **who entered which room and
> what they touched.**

### The 4 types of audit log
| Log type | Records | On by default? | Analogy |
|---|---|---|---|
| **Admin Activity** | Changes to config/resources (create/delete VM, change IAM) | ✅ Always on (free) | The **sign-in book** — always kept |
| **Data Access** | Who **read/wrote data** (e.g., read a bucket object) | ❌ Off by default (can be noisy/costly) | CCTV on the **filing cabinets** |
| **System Event** | Google-initiated actions (e.g., auto-maintenance) | ✅ On | The building's **maintenance log** |
| **Policy Denied** | Access blocked by policy | ✅ On | Log of **rejected entries** at the gate |

### Why they matter
- **Security investigations** — "who deleted that VM?"
- **Compliance** — prove what happened for audits.
- **Troubleshooting** — see what changed right before something broke.

```bash
# Read recent admin-activity logs
gcloud logging read "logName:cloudaudit.googleapis.com%2Factivity" --limit=10
```

> Tip: Audit logs live in **Cloud Logging** (you'll see more logging on Day 9).

---

## Part B — Organization Policy 🚧 (guardrails)

An **Organization Policy** sets **rules about what is allowed to exist or be configured**
across your whole org/folder/project. It restricts *how* resources can be used.

> **Analogy:** Company-wide **house rules** posted at HQ that **every branch must follow** —
> e.g., *"No office may be opened outside the country,"* or *"No one may prop the front
> door open to the public."* Individual managers can't override them.

### IAM vs Org Policy (key difference)
| | **IAM** | **Org Policy** |
|---|---|---|
| Answers | **Who** can do what? | **What** is allowed to exist/be configured? |
| Example | "Alice can create VMs" | "VMs may only be created in `asia-south1`" |
| Analogy | **Who holds which key card** | **The building's house rules** |

> They work together: IAM says Alice *may* create a VM; Org Policy says *only in approved
> regions*. Even an **Owner** can't break an Org Policy constraint.

### Common constraints (examples)
- **Restrict resource locations** — only allow certain regions (data residency).
- **Disable service account key creation** — block risky downloadable keys.
- **Skip default network creation** — force teams to design their own VPC.
- **Restrict public IPs on VMs** — keep VMs private.
- **Restrict allowed external sharing / domains**.

```bash
# List available constraints
gcloud resource-manager org-policies list --project=PROJECT_ID

# Example: enforce "no service account key creation"
gcloud resource-manager org-policies enable-enforce \
  iam.disableServiceAccountKeyCreation --project=PROJECT_ID
```

---

## How they fit together
- **Org Policy** = set the **guardrails** up front (prevent bad configs).
- **IAM** = control **who** can act within those guardrails.
- **Audit Logs** = the **record** to verify and investigate afterwards.
- *Analogy:* **house rules** (org policy) + **key cards** (IAM) + **CCTV/sign-in book**
  (audit logs) = a well-secured building.

---

## TL;DR (in plain English)
- **Audit Logs = CCTV + sign-in book**: who did what, when. **Admin Activity** is always
  on; **Data Access** is off by default (turn on where needed).
- **Org Policy = company-wide house rules** (guardrails) on *what can exist/be configured* —
  even Owners can't override them.
- **IAM = who can act; Org Policy = what's allowed; Audit Logs = the record.** Use all three.
