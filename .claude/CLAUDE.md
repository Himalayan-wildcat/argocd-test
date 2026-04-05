# ArgoCD / Argo Rollouts — Local Testing

## Cluster
- **Kind cluster name:** `kind`
- **Context:** `kind-kind`
- **ArgoCD UI:** http://argocd.localhost (exposed via Kubernetes Gateway API, not Ingress)

## What's installed
- ArgoCD — namespace: `argo-cd`
- Argo Rollouts v1.9.0 — namespace: `argo-rollouts` (2 controller replicas + dashboard)
- Kubernetes Gateway API — implementation: **Cilium** (`HTTPRoute`, `GRPCRoute`, etc.) — Gateway `cilium-gateway` is in `default` namespace
- Gateway API plugin for Argo Rollouts: **not yet configured** (argo-rollouts-config has no data)

## Session goal
Test Argo Rollouts behavior (canary, blue-green, etc.) managed by ArgoCD.

## Manifest approach
Individual `Application` manifests applied directly — no app-of-apps pattern.

## Useful commands
```bash
# Switch to kind context
kubectl config use-context kind-kind

# Watch rollout status
kubectl argo rollouts get rollout <name> -n <namespace> --watch

# Promote a rollout (advance canary/blue-green)
kubectl argo rollouts promote <name> -n <namespace>

# Abort a rollout
kubectl argo rollouts abort <name> -n <namespace>

# Retry a failed rollout
kubectl argo rollouts retry rollout <name> -n <namespace>

# ArgoCD CLI (server reachable via ingress)
argocd app sync <app-name> --server argocd.localhost --grpc-web
argocd app get <app-name> --server argocd.localhost --grpc-web
```

## Directory layout (planned)
```
manifests/
  argocd/
    applications/       ← ArgoCD Application manifests
      <app-name>.yaml
  argo-rollouts/
    workloads/          ← Rollout + Service + etc. manifests (if kept here)
```
