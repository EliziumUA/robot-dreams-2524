## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_8
cd homework_8

## Create files
nano namespace.yaml
nano service.yaml
nano deployment.yaml

## Create namespace
microk8s.kubectl apply -f namespace.yaml
microk8s.kubectl get namespaces

## Create service
microk8s.kubectl apply -n my-namespace -f service.yaml
microk8s.kubectl get services -n my-namespace

## Create deployment
microk8s.kubectl apply -n my-namespace -f deployment.yaml
microk8s.kubectl get deployments -n my-namespace

## Describe deployment && curl
microk8s.kubectl describe deployment -n my-namespace my-deployment
curl http://10.152.183.168

## Delete deployment/service/namespace
microk8s.kubectl delete deployment -n my-namespace my-deployment
microk8s.kubectl delete service -n my-namespace my-service
microk8s.kubectl delete namespace my-namespace
