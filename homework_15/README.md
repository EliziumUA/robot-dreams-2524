## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_11
cd homework_11

## Step 1
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
