## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_11
cd homework_11

## Step 1
microk8s config > ~/.kube/config
microk8s.kubectl get nodes
microk8s.kubectl cluster-info
microk8s.kubectl get all --all-namespaces

nano namespace.yaml
microk8s.kubectl apply -f namespace.yaml

nano tls-clusterissuer.yaml
nano tls-certificate.yaml
microk8s enable cert-manager
microk8s.kubectl apply -f tls-clusterissuer.yaml
microk8s.kubectl apply -f tls-certificate.yaml

echo -n "rootpass" | base64
nano mysql-secret.yaml
microk8s.kubectl apply -f mysql-secret.yaml

nano mysql-pvc.yaml
microk8s.kubectl apply -f mysql-pvc.yaml

nano mysql-configmap.yaml
microk8s.kubectl apply -f mysql-configmap.yaml

nano mysql-deployment.yaml
microk8s.kubectl apply -f mysql-deployment.yaml

nano mysql-service.yaml
microk8s.kubectl apply -f mysql-service.yaml

nano wp-pvc.yaml
microk8s.kubectl apply -f wp-pvc.yaml

nano wordpress-deployment.yaml
microk8s.kubectl apply -f wordpress-deployment.yaml

nano wordpress-service.yaml
microk8s.kubectl apply -f wordpress-service.yaml

nano wordpress-ingress.yaml
microk8s.kubectl apply -f wordpress-ingress.yaml

nano wordpress-hpa.yaml
microk8s enable metrics-server
microk8s.kubectl apply -f wordpress-hpa.yaml
microk8s.kubectl get hpa -n wordpress

sudo apt update && sudo apt install siege -y

curl -k https://wp.local/

siege -c 20 -t 3m http://wp.local

watch microk8s.kubectl get hpa -n wordpress
watch microk8s.kubectl get pods -n wordpress

curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm create my-wordpress

helm dependency update my-wordpress/
helm install wp ./my-wordpress/ --kubeconfig ~/.kube/config -n wordpress --create-namespace

microk8s.kubectl get all -n wordpress
microk8s.kubectl describe ingress -n wordpress
microk8s.kubectl get pvc -n wordpress
