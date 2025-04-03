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

# 1. Видалення ресурсу Ingress
microk8s.kubectl delete ingress wordpress-ingress -n wordpress
# 2. Видалення сервісів
microk8s.kubectl delete svc wordpress -n wordpress
# 3. Видалення деплойментів
microk8s.kubectl delete deployment wordpress -n wordpress
# 4. Видалення PVC та PV (за потреби)
microk8s.kubectl delete pvc wp-pvc -n wordpress
# 5. Видалення секретів (якщо створювали окремо)
microk8s.kubectl delete secret mysql-secret -n wordpress
# 6. Видалення HPA (якщо створювали)
microk8s.kubectl delete hpa wordpress-hpa -n wordpress


microk8s.kubectl create namespace monitoring

helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

helm show values grafana/grafana > grafana-default-values.yaml

nano clusterissuer.yaml
microk8s.kubectl apply -f clusterissuer.yaml

nano grafana-cert.yaml
microk8s.kubectl apply -f grafana-cert.yaml

nano grafana-values.yaml
helm install grafana grafana/grafana \
--namespace monitoring \
--values grafana-values.yaml

microk8s.kubectl get all -n monitoring
microk8s.kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm show values prometheus-community/prometheus > prometheus-default-values.yaml






nano prometheus-cert.yaml
microk8s.kubectl apply -f prometheus-cert.yaml

nano prometheus-values.yaml
helm install prometheus prometheus-community/prometheus \
--namespace monitoring \
--values prometheus-values.yaml

microk8s.kubectl get all -n monitoring

microk8s.kubectl get pods -n monitoring -l app.kubernetes.io/name=node-exporter
microk8s.kubectl get pods -n monitoring -l app.kubernetes.io/name=kube-state-metrics

helm show values grafana/loki > loki-default-values.yaml

nano loki-values.yaml
helm install loki grafana/loki \
--namespace monitoring \
--values loki-values.yaml

helm show values grafana/fluent-bit > fluent-default-values.yaml

nano fluentbit-values.yaml
helm install fluent-bit grafana/fluent-bit \
--namespace monitoring \
--values fluentbit-values.yaml

microk8s.kubectl get pods -n monitoring
microk8s.kubectl get svc -n monitoring

microk8s.kubectl port-forward svc/loki 3100:3100 -n monitoring
curl http://localhost:3100/ready
