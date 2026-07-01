# Day 8 — Containers & Kubernetes (GKE)

Notes for Day 8 of the [10-Day GCP Learning Plan](../GCP-10-Day-Learning-Plan.md).

> **Big-picture analogy:** A **container** is a **standardized shipping container** for
> software — your app plus everything it needs, packed to run the same anywhere.
> **Kubernetes** is the **port + crane system** that loads, arranges, and manages
> thousands of these containers automatically. **GKE** is Google running that whole
> port for you.

## Topics
1. [Containers & Docker](01-containers-and-docker.md) — images, containers, Artifact Registry.
2. [Kubernetes Concepts](02-kubernetes-concepts.md) — pods, deployments, services, and friends.
3. [GKE Basics](03-gke-basics.md) — Autopilot vs Standard, control plane vs nodes.
4. [Scaling & Networking](04-scaling-and-networking.md) — HPA, cluster autoscaler, services & ingress.
5. [GKE Operations](05-gke-operations.md) — rollouts, config/secrets, best practices.

## Day 8 Goals
- [ ] Explain what a container is and how it differs from a VM.
- [ ] Understand core Kubernetes objects (pod, deployment, service).
- [ ] Know the difference between GKE Autopilot and Standard.
- [ ] Understand autoscaling (pods and nodes) and how services expose apps.
- [ ] Create a GKE cluster, deploy an app, expose it, and scale it. **Delete when done.**
