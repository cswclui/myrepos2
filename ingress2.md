# Ingress2



```text
git clone https://github.com/nginxinc/kubernetes-ingress/
cd kubernetes-ingress/deployments
git checkout v1.11.0
kubectl apply -f common/ns-and-sa.yaml
kubectl apply -f rbac/rbac.yaml
kubectl apply -f common/default-server-secret.yaml
kubectl apply -f common/nginx-config.yaml
kubectl apply -f common/ingress-class.yaml

kubectl apply -f common/crds/k8s.nginx.org_virtualservers.yaml
kubectl apply -f common/crds/k8s.nginx.org_virtualserverroutes.yaml
kubectl apply -f common/crds/k8s.nginx.org_transportservers.yaml
kubectl apply -f common/crds/k8s.nginx.org_policies.yaml

kubectl apply -f deployment/nginx-ingress.yaml

kubectl get pods --namespace=nginx-ingress 
 
kubectl create -f service/nodeport.yaml
 
cd ~
kubectl apply -f deployment.yaml
kubectl create -f ingress-rules.yaml
curl -H "Host: my.kubernetes.example" 172.17.0.10/webapp1
curl -H "Host: my.kubernetes.example" 172.17.0.10/webapp2
curl -H "Host: my.kubernetes.example" 172.17.0.10
```

10 kubectl get pods --namespace=nginx-ingress 11 kubectl get all --namespace=nginx-ingress 12 kubectl get ing 13 kubectl get ing --namespace=nginx-ingress 14 kubectl create -f service/nodeport.yaml 15\* kubectl get ing --namespace=nginx-ingres 16 kubectl get all --namespace=nginx-ingress 17 history



1. Clone the Ingress controller repo:
2. Change your working directory to /deployments/helm-chart:

```text
git clone https://github.com/nginxinc/kubernetes-ingress/
cd kubernetes-ingress/deployments/helm-chart
git checkout v1.11.0

```

### Adding the Helm Repository

This step is required if you’re installing the chart via the helm repository.

```text
 helm repo add nginx-stable https://helm.nginx.com/stable
helm repo update

```

### Installing the Chart

To install the chart with the release name my-release \(my-release is the name that you choose\):

For NGINX:

```text

helm install my-release nginx-stable/nginx-ingress
```







## overall



```text
 git clone https://github.com/nginxinc/kubernetes-ingress/
cd kubernetes-ingress/deployments/helm-chart
 git checkout v1.11.0
helm repo add nginx-stable https://helm.nginx.com/stable
 helm repo update
helm install my-release nginx-stable/nginx-ingress
 cd ~
kubectl apply -f deployment.yaml
```

10 kubectl create -f ingress-rules.yaml 11 curl -H "Host: my.kubernetes.example" 172.17.0.10/webapp1 12 kubectl get svc 13 curl -H "Host: my.kubernetes.example" 172.17.0.31/webapp1

## reference

* [https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/)
* 
