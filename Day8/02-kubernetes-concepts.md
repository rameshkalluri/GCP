# Kubernetes Concepts

## What is Kubernetes?
**Kubernetes (K8s)** is an open-source **container orchestrator** — it runs, scales,
heals, and connects many containers across many machines, automatically.

> **Analogy:** If a container is a **shipping container**, Kubernetes is the **automated
> port**: cranes place containers on ships (servers), replace damaged ones, add more
> when cargo piles up, and route trucks to the right dock. You give it a **plan**;
> it makes reality **match the plan**.

---

## Declarative model (the key idea)
You describe the **desired state** ("I want 3 copies of this app running"), and
Kubernetes **continuously works to match it**. If one dies, it starts another.

> **Analogy:** You tell the port manager "**always keep 3 ships docked**." If one
> leaves or sinks, the manager **automatically brings another** — no micromanaging.

---

## Core objects (building blocks)

### Pod
- The **smallest unit** — one (or a few tightly-coupled) containers running together.
- *Analogy:* a **single shipping container** on the dock (usually holds one app).

### Deployment
- Manages a set of identical Pods: keeps N running, handles **rolling updates** & rollbacks.
- *Analogy:* the **manager** who guarantees "always 3 pods" and upgrades them safely.

### ReplicaSet
- The mechanism a Deployment uses to keep the right **number of Pod replicas** (usually you don't touch it directly).

### Service
- A **stable network address** + load balancing for a changing set of Pods (Pods come
  and go with new IPs; the Service stays constant).
- *Analogy:* a **permanent front-desk phone number** that always reaches whoever is on
  duty, even as staff change.

### Namespace
- A way to **partition** a cluster into virtual sub-clusters (e.g., `dev`, `prod`).
- *Analogy:* **separate floors** in the same building.

### ConfigMap & Secret
- **ConfigMap** = non-secret config (settings). **Secret** = sensitive values (passwords, keys).
- *Analogy:* a **settings sheet** vs a **locked envelope**.

### Ingress
- Rules for routing **external HTTP(S)** traffic to Services (path/host based).
- *Analogy:* the **building's main entrance + signage** directing visitors to rooms.

---

## Cluster anatomy
```
Cluster
 ├── Control Plane (the "brain": API server, scheduler, controllers)
 └── Nodes (worker machines / VMs)
        └── Pods
              └── Containers
```
- **Control plane** decides *what runs where* and keeps desired state.
- **Nodes** are the machines that actually run your Pods.

---

## Self-healing & scaling (why K8s is loved)
- **Self-healing** — restarts failed containers, replaces dead Pods, reschedules off dead nodes.
- **Horizontal scaling** — add/remove Pod replicas based on load.
- **Rolling updates & rollbacks** — deploy new versions with zero downtime, revert if broken.

---

## Quick `kubectl` Reference
```bash
kubectl get pods                 # list pods
kubectl get deployments          # list deployments
kubectl get services             # list services
kubectl apply -f app.yaml        # create/update from a manifest (desired state)
kubectl scale deployment web --replicas=5
kubectl rollout status deployment web
kubectl logs POD_NAME
```

---

## TL;DR (in plain English)
- **Kubernetes = an automated port** for containers: you declare the **desired state**,
  it keeps reality matching it (self-healing, scaling, rolling updates).
- Core objects: **Pod** (smallest unit), **Deployment** (keeps N pods + updates),
  **Service** (stable address + LB), **Ingress** (HTTP routing), **ConfigMap/Secret** (config).
- A cluster = **control plane (brain)** + **nodes (workers running pods)**.
