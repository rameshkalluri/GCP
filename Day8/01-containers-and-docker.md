# Containers & Docker

## What is a Container?
A **container** packages your **app + all its dependencies** (libraries, runtime,
config) into one unit that runs **the same everywhere** — your laptop, a VM, or the cloud.

> **Analogy:** A **shipping container**. Before them, cargo was loaded loose and messy.
> Standard containers made it so **any ship, truck, or crane** can handle **any** cargo
> the same way. Software containers do this for apps: "it runs the same everywhere."

---

## Container vs VM

| | **Virtual Machine** | **Container** |
|---|---|---|
| Contains | Full guest **OS** + app | Just app + dependencies (shares host OS kernel) |
| Size | GBs, boots in minutes | MBs, starts in **seconds** |
| Isolation | Strong (full OS) | Lightweight (process-level) |
| Analogy | A **whole house** per family | **Apartments** in one building (shared foundation) |

> Containers are **lighter and faster** than VMs because they **share the host OS
> kernel** instead of each carrying a full OS.

---

## Docker basics 🐳
**Docker** is the most common tool to **build, run, and share** containers.

Key terms:
- **Dockerfile** — the **recipe** to build an image (steps to install & configure).
- **Image** — the **built package** (read-only template).
- **Container** — a **running instance** of an image.
- **Registry** — where images are stored/shared.

> **Analogy:** Dockerfile = **recipe**, Image = **frozen ready-meal**, Container =
> the **meal heated up and served**, Registry = the **supermarket freezer**.

```dockerfile
# Dockerfile (simple Python web app)
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

```bash
# Build, run locally
docker build -t my-app:1.0 .
docker run -p 8080:8080 my-app:1.0
```

---

## Artifact Registry (store images on GCP)
**Artifact Registry** is GCP's managed store for **container images** (and other
packages). It replaced the older **Container Registry (GCR)**.

```bash
# Create a Docker repo
gcloud artifacts repositories create my-repo \
  --repository-format=docker --location=asia-south1

# Auth Docker to push to it
gcloud auth configure-docker asia-south1-docker.pkg.dev

# Tag & push an image
docker tag my-app:1.0 asia-south1-docker.pkg.dev/PROJECT/my-repo/my-app:1.0
docker push asia-south1-docker.pkg.dev/PROJECT/my-repo/my-app:1.0
```

---

## TL;DR (in plain English)
- **Container = a shipping container for software**: app + dependencies, runs the same everywhere.
- **Containers vs VMs:** containers share the host OS kernel → **lighter, faster** (apartments vs houses).
- **Docker:** Dockerfile (recipe) → Image (package) → Container (running) → Registry (store).
- On GCP, store images in **Artifact Registry**, then run them on **Cloud Run** or **GKE**.
