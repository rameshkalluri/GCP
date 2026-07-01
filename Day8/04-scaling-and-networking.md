# Scaling & Networking in GKE

## Two kinds of autoscaling (don't confuse them)

### 1. Horizontal Pod Autoscaler (HPA) — more **pods**
- Adds/removes **Pod replicas** based on CPU, memory, or custom metrics.
- *Analogy:* **more cashiers** when lines get long, fewer when quiet.

### 2. Cluster Autoscaler — more **nodes**
- Adds/removes **nodes (VMs)** when Pods can't fit / nodes are idle.
- *Analogy:* **opening more checkout lanes (physical space)** when all cashiers are full.

> They work together: HPA wants more Pods → if no room, Cluster Autoscaler adds a node.
> (In **Autopilot**, node scaling is automatic — you mostly think about Pods.)

### Bonus: Vertical Pod Autoscaler (VPA)
- Adjusts a Pod's **CPU/memory requests** (right-sizing), rather than the count.

```bash
# HPA: scale web deployment between 2 and 10 pods at 60% CPU
kubectl autoscale deployment web --min=2 --max=10 --cpu-percent=60
```

---

## Exposing apps: Service types

| Type | Reachable from | Analogy |
|---|---|---|
| **ClusterIP** (default) | **Inside** the cluster only | An **internal extension** number |
| **NodePort** | A port on every node | A **side door** on each building |
| **LoadBalancer** | The **internet** (provisions a GCP LB) | The **public entrance** with a street address |
| **Ingress** | HTTP(S), path/host routing to Services | The **main lobby + signage** to departments |

- **ClusterIP** — internal microservice-to-microservice traffic.
- **LoadBalancer** — expose a single service publicly (gets an external IP).
- **Ingress** — one entry point routing `/api`, `/web`, etc. to different Services
  (ties back to Day 4 load balancing; GKE Ingress provisions a Google LB).

```yaml
# service.yaml — expose "web" to the internet
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  type: LoadBalancer
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 8080
```

---

## Networking notes
- Each Pod gets its **own IP**; Pods talk to each other directly within the cluster.
- **Services** provide stable addresses in front of changing Pods (Pods are ephemeral).
- **Namespaces** isolate environments; **Network Policies** restrict Pod-to-Pod traffic
  (like firewall rules inside the cluster).
- GKE clusters live in a **VPC/subnet** (Day 4) — networking concepts carry over.

---

## Health checks (probes)
- **Liveness probe** — is the container alive? If not, **restart** it.
- **Readiness probe** — is it ready to receive traffic? If not, **don't route** to it yet.
- *Analogy:* liveness = "is the cashier conscious?"; readiness = "is the lane **open**
  for customers?"

---

## TL;DR (in plain English)
- **HPA scales Pods**, **Cluster Autoscaler scales Nodes** — together they handle load
  (Autopilot does node scaling for you).
- Expose apps with **Services**: **ClusterIP** (internal), **LoadBalancer** (public IP),
  **Ingress** (HTTP routing to many services).
- Pods are ephemeral with their own IPs; **Services** give stable addresses. Use
  **liveness/readiness probes** for healthy traffic routing.
