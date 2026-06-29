# Billing & Cost Management

## How GCP Billing is Structured

```
Billing Account  ──(linked to)──►  Project(s)  ──►  Resources (incur charges)
```

- A **Billing Account** holds payment info and pays for usage.
- One billing account can be linked to **many projects**.
- A project **must** be linked to a billing account to use paid services.

### Billing account types
- **Self-serve (online)** — pay by credit/debit card; charged automatically.
- **Invoiced (offline)** — invoice-based billing for larger/enterprise customers.

---

## Free Tier & Credits
- **$300 free credit** for new accounts (valid ~90 days).
- **Always Free** tier — limited usage of some products free every month
  (e.g., small Compute Engine `e2-micro`, Cloud Storage, Cloud Functions limits).
- Credits are consumed first; you won't be charged until they run out **and** you
  upgrade to a paid account.

---

## Controlling Cost (do this on Day 1!)

### Budgets & Alerts
- Set a **budget** on a billing account or project.
- Configure **alert thresholds** (e.g., 50%, 90%, 100%) → email notifications.
- Budgets **alert** you; they do **not** automatically stop spending.
  - To enforce hard stops, wire budget alerts to **Pub/Sub → Cloud Function** that
    disables billing or shuts down resources.

### Other cost tools
- **Billing Reports** — visualize spend by project/service/SKU.
- **Cost Table & Cost Breakdown** — detailed line items.
- **Billing Export to BigQuery** — analyze detailed usage with SQL.
- **Pricing Calculator** — estimate costs before deploying.
- **Quotas** — limit resource usage to prevent runaway costs.

---

## Ways to Save Money
- **Sustained use discounts** — automatic for steady Compute Engine usage.
- **Committed use discounts (CUDs)** — commit to 1–3 yr usage for big savings.
- **Spot / Preemptible VMs** — up to ~60–91% cheaper (can be reclaimed anytime).
- **Right-sizing** — use recommendations to downsize over-provisioned VMs.
- **Autoscaling** — scale to zero where possible (Cloud Run, functions).
- **Storage classes / lifecycle** — move cold data to Nearline/Coldline/Archive.
- **Delete idle resources** — stop/delete VMs, unused IPs, disks, snapshots.

---

## Daily Cost-Safety Checklist
- [ ] Budget + alert set on the billing account.
- [ ] Stop/delete VMs after labs.
- [ ] Release unused static external IPs.
- [ ] Delete unused disks/snapshots/buckets.
- [ ] Check the Billing dashboard.

---

## Quick `gcloud` Reference

```bash
# List billing accounts
gcloud billing accounts list

# Link a project to a billing account
gcloud billing projects link PROJECT_ID --billing-account=BILLING_ACCOUNT_ID

# Show a project's billing info
gcloud billing projects describe PROJECT_ID

# Disable billing on a project (stops paid resource usage)
gcloud billing projects unlink PROJECT_ID
```

---

## TL;DR
- **Billing account → projects → resources.**
- New users get **$300 credit** + an **Always Free** tier.
- **Set a budget + alert on Day 1** (alerts notify, they don't auto-stop).
- Save with sustained/committed discounts, Spot VMs, right-sizing, and lifecycle rules.
- **Delete idle resources** to avoid surprise charges.
