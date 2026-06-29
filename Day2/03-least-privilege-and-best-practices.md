# Least Privilege & Security Best Practices

## What is "Least Privilege"?
Give every person or app **only the access they need to do their job — and nothing more.**

> **Analogy:** You don't give the **pizza delivery person** the keys to your whole
> house. You just open the **front door** long enough to take the pizza. Same idea in
> the cloud: give the **smallest** access needed, for the **shortest** time.

---

## Why it matters
- **Smaller blast radius:** if an account is hacked, the damage is limited.
  - *Analogy:* if a thief steals the **mailbox key**, they can't open the **safe**.
- **Fewer mistakes:** people can't accidentally break things they can't touch.
- **Easier audits:** clear who can do what.

---

## The Layers of Security (defense in depth 🧅)

Think of security like an **onion** — many layers, so one failure isn't fatal:

1. **Identity** — strong logins, **2-step verification (MFA)** for humans.
2. **IAM roles** — least privilege (predefined > primitive; custom when needed).
3. **Resource boundaries** — separate **projects** for dev/prod (don't mix).
4. **Org Policies** — company-wide guardrails (e.g., "only allow region X").
5. **Network** — firewalls, private IPs (covered more on Day 4).
6. **Data** — encryption (on by default) + secrets in **Secret Manager**.
7. **Monitoring** — **audit logs** to see who did what.

---

## Practical Best Practices (checklist)

### Identities & access
- ✅ Use **groups** for people (`developers@`), not one-by-one grants.
- ✅ Prefer **predefined roles**; avoid `Owner`/`Editor` in production.
- ✅ Use **custom roles** when predefined ones are too broad.
- ✅ Turn on **MFA / 2-step verification** for all human accounts.
- ✅ Review access regularly with the **IAM Recommender**.

### Service accounts
- ✅ One SA per workload, least privilege.
- ✅ Prefer **Workload Identity** over key files.
- ✅ Rotate/delete unused keys; never store keys in Git.

### Projects & org
- ✅ Separate **dev / staging / prod** into different projects.
- ✅ Set **Org Policy constraints** (allowed regions, services, etc.).
- ✅ Set **budgets + alerts** (a runaway bill is a security/ops issue too).

### Secrets & data
- ✅ Store passwords/keys in **Secret Manager**, not in code.
- ✅ Rely on **default encryption** at rest and in transit.

### Visibility
- ✅ Keep **Cloud Audit Logs** on — they answer "who did what, when?"

---

## Common Mistakes to Avoid ❌
- Giving everyone `Owner` "to make things work."
- Sharing one service account across many apps.
- Downloading SA keys and emailing/committing them.
- Using `allUsers` (public) access by accident on a bucket.
- One giant project holding dev + prod together.

---

## IAM Conditions (bonus: time/context-based access)
You can make a key card work **only sometimes** or **only on certain things**.

> *Analogy:* a hotel key card that **only works 9am–5pm**, or **only on the 3rd floor**.

Examples:
- Grant access **only until a certain date** (temporary contractor).
- Grant access **only to resources with a certain name/tag**.

---

## TL;DR (in plain English)
- **Least privilege** = only the access needed, nothing extra (don't give the pizza guy your house keys).
- Security is an **onion**: identity → IAM → projects → org policy → network → data → logs.
- Use **groups, predefined/custom roles, MFA, separate prod, Secret Manager, audit logs**.
- Avoid `Owner`-for-everyone, shared SAs, loose keys, and accidental public access.
