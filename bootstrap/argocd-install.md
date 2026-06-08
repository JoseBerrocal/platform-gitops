# ArgoCD Bootstrap

## Create Namespace

```bash
kubectl create namespace argocd
```

## Install ArgoCD

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Verify Installation

```bash
kubectl get pods -n argocd
```

Wait until all pods are:

```text
Running
```

## Verify CRDs

```bash
kubectl get crd | grep argoproj
```

Expected:

```text
applications.argoproj.io
applicationsets.argoproj.io
appprojects.argoproj.io
```

## Verify Services

```bash
kubectl get svc -n argocd
```

Expected:

```text
argocd-server
argocd-repo-server
argocd-application-controller
```

## Get Initial Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

## Login

User:

```text
admin
```

Password:

```text
<output from previous command>
```

## Next Step

Deploy platform applications:

```bash
kubectl apply -f apps/ingress-nginx.yaml

kubectl apply -f apps/monitoring.yaml
```