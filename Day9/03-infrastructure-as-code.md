# Infrastructure as Code (IaC)

## What is IaC?
**Infrastructure as Code** means defining your cloud resources (VMs, networks, buckets,
databases) in **code/config files** instead of clicking in the Console — so infra is
**repeatable, versioned, and automated**.

> **Analogy:** Instead of building a house by **hand each time** (clicking buttons),
> you write **blueprints**. Anyone can rebuild the **exact same house**, review changes,
> and keep a **history of every edit**.

---

## Why IaC? (the benefits)
- **Repeatable** — spin up identical dev/staging/prod environments.
- **Version-controlled** — infra changes live in Git (review, history, rollback).
- **Auditable** — see exactly what changed, when, and by whom.
- **Automatable** — plug into CI/CD pipelines.
- **Less human error** — no forgotten manual step.

> **Analogy:** clicking in the Console is like **cooking from memory** (inconsistent);
> IaC is a **written recipe** anyone can follow to get the same dish every time.

---

## Declarative vs Imperative
- **Declarative** (preferred) — you describe the **desired end state**; the tool figures
  out how to get there (Terraform, Kubernetes).
- **Imperative** — you write **step-by-step commands** (gcloud scripts).
- *Analogy:* declarative = "**I want a 3-bedroom house**"; imperative = "**lay this
  brick, then that brick...**"

---

## The main tools on GCP

### Terraform (industry standard) ⭐
- Open-source, by HashiCorp; **multi-cloud**; huge community.
- Uses **HCL** config; tracks a **state file** of what it created.
- Core loop: **`init` → `plan` (preview) → `apply` (make it real)**.

```hcl
# main.tf — create a VM with Terraform
resource "google_compute_instance" "vm" {
  name         = "tf-vm"
  machine_type = "e2-medium"
  zone         = "asia-south1-a"
  boot_disk { initialize_params { image = "debian-cloud/debian-12" } }
  network_interface { network = "default" }
}
```

```bash
terraform init      # download providers
terraform plan      # preview changes (dry run)
terraform apply     # create/update resources
terraform destroy   # tear everything down
```

### Other options
- **Google Cloud Deployment Manager** — Google's native IaC (YAML/Jinja/Python); less
  common now that Terraform dominates.
- **Config Connector / Config Controller** — manage GCP resources as **Kubernetes**
  objects.
- **Pulumi** — IaC using real programming languages (Python, TS, Go).

---

## `terraform plan` — the safety net
`plan` shows a **preview/diff** of what will be created, changed, or destroyed **before**
you apply.

> **Analogy:** a **"before you build" review** of the blueprint changes — catch a
> demolition you didn't intend **before** the bulldozer arrives.

---

## Best practices
- Store code in **Git**; require **plan review** in pull requests.
- Keep **state** in a remote backend (e.g., a **GCS bucket**) with locking.
- Never hardcode secrets — use **Secret Manager**/variables.
- Use **modules** to reuse infra patterns; separate **dev/staging/prod**.

---

## TL;DR (in plain English)
- **IaC = infrastructure defined in code** → repeatable, versioned, auditable, automatable.
- Prefer **declarative** (describe the end state). **Terraform** is the standard:
  **init → plan → apply**.
- Always review **`terraform plan`** before applying; store **state remotely**, keep
  code in **Git**, and keep **secrets out** of the config.
