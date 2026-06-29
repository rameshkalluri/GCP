# Cloud Console, gcloud CLI & Cloud Shell

Three main ways to interact with GCP: the **web Console**, the **gcloud CLI**, and
the browser-based **Cloud Shell**.

---

## 1. Cloud Console (web UI)
- URL: <https://console.cloud.google.com>
- Visual management of all resources.
- Key elements:
  - **Project selector** (top bar) — switch active project.
  - **Navigation menu** (☰) — browse all services.
  - **Search bar** — jump to any resource/service/setting.
  - **Cloud Shell icon** (`>_`) — open a terminal in the browser.
  - **IAM & Admin, Billing, APIs & Services** sections.
- Best for: exploring, one-off tasks, learning, dashboards.

---

## 2. gcloud CLI (Cloud SDK)
The command-line tool to manage GCP. Install locally or use it pre-installed in Cloud Shell.

### Setup
```bash
gcloud init                 # interactive setup (login + project + region)
gcloud auth login           # authenticate your account
gcloud config list          # show active config (account, project, region)
```

### Configuration
```bash
gcloud config set project PROJECT_ID
gcloud config set compute/region asia-south1
gcloud config set compute/zone asia-south1-a
gcloud config configurations list   # manage multiple named configs
```

### Components
- **gcloud** — core CLI for most services.
- **gsutil** — Cloud Storage (older; `gcloud storage` is the newer equivalent).
- **bq** — BigQuery CLI.
- **kubectl** — Kubernetes (via `gcloud components install kubectl`).

```bash
gcloud components list
gcloud components update
```

### Handy patterns
```bash
gcloud projects list
gcloud compute instances list
gcloud <group> <command> --help     # built-in help
gcloud compute instances list --format=json
gcloud compute instances list --filter="zone:asia-south1-a"
```

---

## 3. Cloud Shell
A free, browser-based Linux VM with the SDK and tools pre-installed.

- **No setup** — gcloud, gsutil, bq, kubectl, Docker, Python, etc. ready to go.
- **5 GB persistent home directory** (`$HOME` survives sessions).
- **Already authenticated** as your console user.
- **Cloud Shell Editor** — built-in VS Code-like editor.
- **Web Preview** — preview web apps on a port.
- Ephemeral VM (resets after inactivity) but `$HOME` persists.

Best for: quick commands, labs, and learning without local installation.

---

## Console vs CLI — When to Use
| Use the Console when... | Use the CLI when... |
|---|---|
| Exploring/learning | Automating/scripting |
| One-off visual tasks | Repeatable, reproducible tasks |
| Viewing dashboards/graphs | CI/CD pipelines, bulk operations |

---

## TL;DR
- **Console** = web UI for exploring and visual tasks.
- **gcloud CLI** = scriptable command line; configure with `gcloud init`.
- **Cloud Shell** = zero-setup browser terminal, pre-authenticated, 5 GB persistent home.
- Learn `gcloud config`, `--help`, `--format`, and `--filter` early.
