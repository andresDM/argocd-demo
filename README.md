## Start Minikube

```
minikube start
```

## Install ArgoCD

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Access ArgoCD

```
kubectl get pod -n argocd

kubectl get svc -n argocd

kubectl port-forward -n argocd svc

kubectl port-forward -n argocd svc/argocd-server 8080:443
```

User: admin
Password:
```
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml | grep password | awk '{print $2}' | base64 -d
```


We need to apply argo cd config manually(for now, there is autodiscovery):
```
kubectl apply -f application.yaml
kubectl apply -f chart-museum.yaml
```

## Monitoring

```
minikube dashboard
```


## Access ChartMuseum

```
kubectl port-forward svc/chart-museum-argo-application-chartmuseum 8150:8080 -n chartmuseum
```


docker run --rm -it \
-p 8150:8080 \
-e DEBUG=1 \
-e STORAGE=local \
-e STORAGE_LOCAL_ROOTDIR=/charts \
-v $(pwd)/charts:/charts \
ghcr.io/helm/chartmuseum:v0.14.0



