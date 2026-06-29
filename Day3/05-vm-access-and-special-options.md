# VM Access & Special Options

Extra Compute Engine topics that round out Day 3: **how you log in securely**, **how to
make VMs more secure**, and **special hardware/placement options**.

---

## 1. Logging in: OS Login vs SSH Keys 🔑

To get into a Linux VM you need SSH access. There are two ways to manage who can log in.

### Metadata SSH keys (the old/manual way)
- You add public SSH keys to the VM (or project) metadata.
- *Analogy:* taping a **list of allowed keys to the door** of each house — works, but
  managing many keys across many VMs is messy.

### OS Login (the recommended way) ✅
- Ties SSH access to a user's **Google identity + IAM roles**.
- Add/remove a person in **IAM** → their VM access updates everywhere automatically.
- *Analogy:* a **central badge system** for the whole building — revoke someone's badge
  once and they're locked out of **every** room instantly.

| | Metadata SSH keys | OS Login |
|---|---|---|
| Managed via | Keys in metadata | **IAM identities/roles** |
| Scale | Hard (per-key) | Easy (central) |
| Revoke access | Remove each key | Remove **one IAM role** |
| Analogy | Key list on each door | Central badge system |

```bash
# Turn on OS Login project-wide
gcloud compute project-info add-metadata --metadata enable-oslogin=TRUE
```

> Roles: `roles/compute.osLogin` (normal) and `roles/compute.osAdminLogin` (sudo).

---

## 2. Shielded VMs 🛡️
**Shielded VMs** add protections that make sure the VM **boots clean and untampered**
(secure boot, vTPM, integrity monitoring).

> **Analogy:** A **tamper-proof seal** on a medicine bottle — if someone messed with the
> boot process/rootkit, you'll know the seal is broken.

- Good default for security-sensitive workloads (many images enable it by default).
- **Confidential VMs** go further: they **encrypt data even while in use (in memory)**.
  - *Analogy:* working inside a **soundproof, locked booth** — even the building owner
    can't peek at what you're processing.

---

## 3. Special placement & hardware

### Sole-tenant nodes
- Your VMs run on a **physical server dedicated to you** (not shared with other customers).
- *Analogy:* renting an **entire floor to yourself** instead of a shared co-working desk —
  for compliance/licensing needs.

### GPUs & TPUs (accelerators)
- **GPUs** — add graphics/AI processing power to a VM (ML training, rendering).
- **TPUs** — Google's custom chips built specifically for machine learning.
- *Analogy:* bolting a **turbocharger** (GPU) or a **purpose-built race engine** (TPU)
  onto your rented car for heavy AI jobs.

### Reservations
- **Reserve** capacity in advance so VMs are guaranteed available when you need them.
- *Analogy:* **booking tables** at a restaurant ahead of a big rush.

---

## 4. Quick security/access checklist
- ✅ Prefer **OS Login** over scattered SSH keys.
- ✅ Limit SSH (port 22) firewall access to **known IPs** (Day 4).
- ✅ Use **Shielded VM** images; **Confidential VM** for sensitive in-memory data.
- ✅ Give VMs a **least-privilege service account** (Day 2), not broad default access.
- ✅ Keep VMs **private** (no public IP) + **Cloud NAT/IAP** where possible (Day 4).

---

## TL;DR (in plain English)
- **OS Login** = manage SSH access through **IAM identities** (central badge system) —
  prefer it over per-VM **metadata SSH keys** (key list on each door).
- **Shielded VM** = tamper-proof boot seal; **Confidential VM** = data encrypted even
  while in use.
- **Sole-tenant nodes** = a dedicated physical server just for you.
- **GPUs/TPUs** = turbocharger/race-engine for AI & heavy compute; **reservations** =
  book capacity ahead.
