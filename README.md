## Install argocd on EKS Cluster

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

# Add a namespace label to instruct Istio to automatically inject Envoy sidecar proxies when you deploy your application later
```bash
kubectl label namespace argocd  istio-injection=enabled
kubectl get namespace -L istio-injection

```

# Tells argocd-server to start in “insecure mode” and patched argocd-server deployment

```bash

# kubectl edit cm argocd-cmd-params-cm
# and add server.insecure option

data:
  server.insecure: "true"
  
Then restart the argocd-server deployment
```

