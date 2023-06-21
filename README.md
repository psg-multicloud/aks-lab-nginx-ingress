# aks-lab-nginx-ingress
Laboratório para provisionamento Azure Kubernetes Services com Nginx Ingress Controller
EM CONSTRUÇÃO

Objetivo : Criar e configurar o AKS para ambiente produtivo usando o NGINX como Ingress Controller para acesso externo da aplicação.

Tecnologias envolvidas : NGINX, Azure Kubernetes Service.

# Criar namespace para alocar os recursos do ingress controller
kubectl create namespace ingress

# Adicionar repositorio do ingress nginx

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add stable https://charts.helm.sh/stable/
helm repo update

#Criar load balancer internal AKS 

apiVersion: v1
kind: Service
metadata:
  name: lb-internal
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:kuj
    app: lb-internal

ip do load balance internal
ip=

# Fazer o Deploy do NGINX ingress controller

helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress
  --set controller.replicaCount=1 \
  --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set controller.service.externalTrafficPolicy=Local \
  --set controller.service.loadBalancerIP="$ip"
