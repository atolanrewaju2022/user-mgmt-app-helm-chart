#Test deploying to kubernetes cluster before setting up the pipeline
#Change directory to helm chart folder
```bash
cd  user-mgmt-app-helm-chart/helm/nodejs-user-app
```

#Install the chart with a release name called account-manager 
```bash
helm install account-manager  .  -f dev-values.yaml
```

#Upgrade the Helm Chart with new image 
```bash
helm upgrade --namespace $NAMESPACE  -f  ./helm/account_manager/dev-values.yaml --set image.repository=$ECR_REGISTRY/$DOCKER_TAG  --set image.tag=$BITBUCKET_COMMIT --install $HELM_CHART $HELM_RELEASE
```

#Install argocd on EKS Cluster

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

#Add a namespace label to instruct Istio to automatically inject Envoy sidecar proxies when you deploy your application later
```bash
kubectl label namespace argocd  istio-injection=enabled
kubectl get namespace -L istio-injection

```

#Configure argocd-server to start in “insecure mode” and patched argocd-server deployment

```bash

# kubectl edit cm argocd-cmd-params-cm
# and add server.insecure option

data:
  server.insecure: "true"
  
Then restart the argocd-server deployment
```

