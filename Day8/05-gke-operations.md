# GKE Operations

Running apps on GKE day-to-day: **deploying, updating, config/secrets, and best practices.**

---

## Deploying an app (the typical flow)
```
Build image → push to Artifact Registry → write manifests (Deployment + Service)
→ kubectl apply → GKE runs pods → expose via Service/Ingress
```

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 3
  selector:
    matchLabels: { app: web }
  template:
    metadata:
      labels: { app: web }
    spec:
      containers:
        - name: web
          image: asia-south1-docker.pkg.dev/PROJECT/my-repo/my-app:1.0
          ports:
            - containerPort: 8080
          resources:
            requests: { cpu: "100m", memory: "128Mi" }
            limits:   { cpu: "500m", memory: "256Mi" }
```

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## Rolling updates & rollbacks 🔄
- **Rolling update** — replace Pods **gradually** (new ones up before old ones down) →
  **zero downtime**.
  - *Analogy:* retrain staff **one at a time** so the shop never closes.
- **Rollback** — if the new version is broken, revert instantly to the previous one.

```bash
# Update the image (triggers a rolling update)
kubectl set image deployment/web web=.../my-app:2.0
kubectl rollout status deployment/web
kubectl rollout undo deployment/web      # roll back if broken
```

---

## Config & Secrets
- **ConfigMap** — non-sensitive settings (feature flags, URLs) injected as env vars/files.
- **Secret** — sensitive values (passwords, API keys). For stronger security, integrate
  **Secret Manager** / use **Workload Identity** (below).
- *Analogy:* ConfigMap = a **settings sheet**; Secret = a **locked envelope**.

```bash
kubectl create configmap app-config --from-literal=API_URL=https://api.example.com
kubectl create secret generic db-secret --from-literal=password=SECRET
```

---

## Resource requests & limits (important!)
- **Requests** = guaranteed minimum (used for scheduling).
- **Limits** = hard maximum (container is throttled/killed if exceeded).
- Setting these enables **efficient packing** and **autoscaling** to work well.
- *Analogy:* tell the port how much **space each container needs** so it can arrange them.

---

## Security best practices
- **Workload Identity** — let Pods act as a **GCP service account** without key files
  (ties back to Day 2). Preferred over mounting SA keys.
- Use **least-privilege** IAM + Kubernetes **RBAC**.
- Keep nodes/cluster **auto-upgraded**; use **private clusters** where possible.
- Scan images for vulnerabilities; use **Network Policies** to limit Pod traffic.

---

## Observability (ties to Day 9)
- GKE integrates with **Cloud Logging** and **Cloud Monitoring** automatically.
- `kubectl logs`, `kubectl describe`, and `kubectl get events` are your first debugging stops.

```bash
kubectl logs deployment/web
kubectl describe pod POD_NAME
kubectl get events --sort-by=.lastTimestamp
```

---

## Cost & cleanup
- **Delete clusters** after labs (biggest cost).
- Right-size **requests/limits**; use **Autopilot** to avoid paying for idle nodes.
- Remove unused **LoadBalancers** (they hold external IPs that cost money).

---

## TL;DR (in plain English)
- Deploy via **manifests** (`Deployment` + `Service`) and `kubectl apply`; update with
  **rolling updates** and **rollback** if broken (zero downtime).
- Use **ConfigMap/Secret** for config, and set **resource requests/limits** so scheduling
  and autoscaling work.
- Prefer **Workload Identity** + least-privilege RBAC; GKE auto-integrates with **Cloud
  Logging/Monitoring**. **Delete clusters and LBs** after labs.
