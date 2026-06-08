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

## Configure ArgoCD for Local HTTP Ingress

For this local Docker Desktop lab, ArgoCD is exposed through Ingress using HTTP.

Patch ArgoCD to disable HTTPS redirect:

```bash
kubectl patch configmap argocd-cmd-params-cm \
-n argocd \
--type merge \
-p '{"data":{"server.insecure":"true"}}'
```

Restart ArgoCD server:

```bash
kubectl rollout restart deployment argocd-server -n argocd
kubectl rollout status deployment argocd-server -n argocd
```

This allows access through:

```text
http://argocd.local
```

and CLI login without port-forward:

```bash
argocd login argocd.local --insecure
```

## Troubleshooting

### ArgoCD CLI still connects to localhost:8080

Check:

```bash
env | grep ARGOCD
```

If you see:

```text
ARGOCD_SERVER=localhost:8080
```

remove it:

```bash
unset ARGOCD_SERVER
```

or update it:

```bash
export ARGOCD_SERVER=argocd.local
```

Verify:

```bash
argocd app list
```