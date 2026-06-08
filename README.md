# Platform GitOps

GitOps repository for platform components managed through ArgoCD.

## Components

- ArgoCD
- Ingress NGINX
- Prometheus
- Grafana

---

## Repository Structure

```text
platform-gitops/
├── bootstrap
│   └── argocd-install.md
├── apps
│   ├── ingress-nginx.yaml
│   ├── argocd-ingress.yaml
│   ├── monitoring.yaml
│   └── monitoring-ingress.yaml
├── values
│   ├── ingress-nginx-values.yaml
│   └── kube-prometheus-stack-values.yaml
└── README.md
```

---

## Bootstrap

Follow:

```text
bootstrap/argocd-install.md
```

---

## Deploy Applications

```bash
kubectl apply -f apps/ingress-nginx.yaml

kubectl apply -f apps/monitoring.yaml
```

---

## Deploy Ingresses

```bash
kubectl apply -f apps/argocd-ingress.yaml

kubectl apply -f apps/monitoring-ingress.yaml
```

---

## Verification

```bash
kubectl get applications -n argocd

kubectl get pods -n ingress-nginx

kubectl get pods -n observability

kubectl get ingress -A
```

---

## Access URLs

```text
http://argocd.local
http://grafana.local
http://prometheus.local
```

---

## DNS Configuration

Add to:

```bash
/etc/hosts
```

```text
127.0.0.1 argocd.local
127.0.0.1 grafana.local
127.0.0.1 prometheus.local
```