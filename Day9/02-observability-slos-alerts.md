# Observability: SLIs, SLOs & Alerts

## Observability vs Monitoring (quick distinction)
- **Monitoring** = watching **known** metrics you set up in advance.
- **Observability** = being able to **ask new questions** and understand **why**
  something is wrong from the data (metrics + logs + traces together).

> **Analogy:** Monitoring is the **dashboard warning lights**; observability is being
> able to **pop the hood and diagnose any new problem** using all the sensors.

---

## SLIs, SLOs, SLAs (the reliability vocabulary)

| Term | Meaning | Analogy |
|---|---|---|
| **SLI** (Indicator) | A **measured** number (e.g., % of requests under 200ms) | The **thermometer reading** |
| **SLO** (Objective) | Your **internal target** for an SLI (e.g., 99.9% success) | The **goal temperature** you aim for |
| **SLA** (Agreement) | A **contract** with customers (+ penalties if missed) | The **promise** with a refund clause |

- Common SLIs: **availability**, **latency**, **error rate**, **throughput**.
- **SLO** example: "99.9% of requests succeed over 30 days."

---

## Error Budget 💰
If your SLO is **99.9%**, you're allowed **0.1%** failures — that's your **error budget**.

> **Analogy:** A **monthly allowance** for failure. Spend it wisely: if it's used up,
> **slow down risky launches** and focus on stability; if plenty remains, you can ship faster.

---

## Alerting — get told when it matters 🔔
An **alerting policy** watches a metric and **notifies** you (email, SMS, Slack,
PagerDuty) when a **condition** is met (e.g., error rate > 5% for 5 minutes).

Good alerting practices:
- Alert on **symptoms users feel** (high latency, errors), not every tiny blip.
- Avoid **alert fatigue** — too many noisy alerts get ignored.
- Tie alerts to **SLOs** (alert when you're **burning error budget** too fast).

```bash
# Uptime checks + alerting policies are usually set in the Console,
# but notification channels can be managed via gcloud:
gcloud beta monitoring channels list
```

---

## Dashboards
- Build **dashboards** combining the key metrics for a service (latency, traffic,
  errors, saturation — the "**four golden signals**").
- *Analogy:* the **single wall of screens** in the control room showing everything at a glance.

> **Four golden signals:** **Latency, Traffic, Errors, Saturation** — a great default
> set to watch for any service.

---

## TL;DR (in plain English)
- **SLI** = a measured number; **SLO** = your target for it; **SLA** = the customer
  contract (with penalties).
- **Error budget** = allowed failure under your SLO — use it to balance **shipping vs stability**.
- **Alerts** should fire on **user-facing symptoms** and SLO burn, not noise.
- Build **dashboards** around the **four golden signals**: latency, traffic, errors, saturation.
