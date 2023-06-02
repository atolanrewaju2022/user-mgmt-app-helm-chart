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

# Download istio
```bash 
curl -L https://istio.io/downloadIstio | sh -
# Move to the Istio package directory. For example, if the package is istio-1.16.2:
cd istio-1.16.2

# Add the istioctl client to your path (Linux or macOS)
export PATH=$PWD/bin:$PATH

# Istio installation
istioctl install 

```
# Deploy istio Gateway and Virtualservice

```bash
kubectl apply -f istio-gateway.yml   
kubectl apply -f istio-virtualservice.yml 

kubectl get gateway -n argocd 
kubectl get virtualservice -n argocd 
