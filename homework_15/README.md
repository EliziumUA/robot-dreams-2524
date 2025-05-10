## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_15
cd homework_15

## Part 1
### Configuring local kubectl to work with microk8s.
microk8s config > ~/.kube/config
microk8s.kubectl get nodes
microk8s.kubectl cluster-info
microk8s.kubectl get all --all-namespaces

### Create Namespace
nano namespace.yaml
microk8s.kubectl apply -f namespace.yaml

### Create ClusterIssuer && Certificate
nano tls-clusterissuer.yaml
nano tls-certificate.yaml
microk8s enable cert-manager
microk8s.kubectl apply -f tls-clusterissuer.yaml
microk8s.kubectl apply -f tls-certificate.yaml

### Create Mysql secret
echo -n "rootpass" | base64
nano mysql-secret.yaml
microk8s.kubectl apply -f mysql-secret.yaml

### Create Mysql pvc
nano mysql-pvc.yaml
microk8s.kubectl apply -f mysql-pvc.yaml

### Create Mysql configmap
nano mysql-configmap.yaml
microk8s.kubectl apply -f mysql-configmap.yaml

### Create Mysql deployment
nano mysql-deployment.yaml
microk8s.kubectl apply -f mysql-deployment.yaml

### Create Mysql service
nano mysql-service.yaml
microk8s.kubectl apply -f mysql-service.yaml

### Create WordPress pvc
nano wp-pvc.yaml
microk8s.kubectl apply -f wp-pvc.yaml

### Create WordPress deployment
nano wordpress-deployment.yaml
microk8s.kubectl apply -f wordpress-deployment.yaml

### Create WordPress service
nano wordpress-service.yaml
microk8s.kubectl apply -f wordpress-service.yaml

### Create WordPress ingress
nano wordpress-ingress.yaml
microk8s.kubectl apply -f wordpress-ingress.yaml

### Configuring HorizontalPodAutoscaler (HPA) for WordPress in Kubernetes.
nano wordpress-hpa.yaml
microk8s enable metrics-server
microk8s.kubectl apply -f wordpress-hpa.yaml
microk8s.kubectl get hpa -n wordpress

### Installing the Siege utility
sudo apt update && sudo apt install siege -y

### Checking access to WordPress
curl -k https://wp.local/

### Conducting a load test
siege -c 20 -t 3m http://wp.local

### Monitoring HPA response
watch microk8s.kubectl get hpa -n wordpress
watch microk8s.kubectl get pods -n wordpress

## Part 2
### Creating a Helm chart for WordPress with MySQL as a dependency
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm create my-wordpress

### Adding MySQL dependency
helm dependency update my-wordpress/

### Installing the Helm chart
helm install wp ./my-wordpress/ --kubeconfig ~/.kube/config -n wordpress --create-namespace

### Verifying the results
microk8s.kubectl get all -n wordpress
microk8s.kubectl describe ingress -n wordpress
microk8s.kubectl get pvc -n wordpress

### Deleting the Ingress resource
microk8s.kubectl delete ingress wordpress-ingress -n wordpress
### Deleting services
microk8s.kubectl delete svc wordpress -n wordpress
### Deleting deployments
microk8s.kubectl delete deployment wordpress -n wordpress
### Deleting PVC and PV
microk8s.kubectl delete pvc wp-pvc -n wordpress
### Deleting secrets
microk8s.kubectl delete secret mysql-secret -n wordpress
### Deleting HPA
microk8s.kubectl delete hpa wordpress-hpa -n wordpress

### Creating a namespace for monitoring
microk8s.kubectl create namespace monitoring

### Adding the official Grafana Helm repository
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

### Reviewing the chart parameters
helm show values grafana/grafana > grafana-default-values.yaml

### Create ClusterIssuer
nano clusterissuer.yaml
microk8s.kubectl apply -f clusterissuer.yaml

### Create Certificate
nano grafana-cert.yaml
microk8s.kubectl apply -f grafana-cert.yaml

### Create a configuration file
nano grafana-values.yaml
helm install grafana grafana/grafana \
--namespace monitoring \
--values grafana-values.yaml

### Verifying the results
microk8s.kubectl get all -n monitoring

### Retrieving the password
microk8s.kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

#### Adding the official Helm repository for Prometheus
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

### Viewing the Helm chart parameters
helm show values prometheus-community/prometheus > prometheus-default-values.yaml

### Create Certificate
nano prometheus-cert.yaml
microk8s.kubectl apply -f prometheus-cert.yaml

### Configuring the values file
nano prometheus-values.yaml
helm install prometheus prometheus-community/prometheus \
--namespace monitoring \
--values prometheus-values.yaml

### Verifying the results
microk8s.kubectl get all -n monitoring

### Checking Node Exporter and kube-state-metrics
microk8s.kubectl get pods -n monitoring -l app.kubernetes.io/name=node-exporter
microk8s.kubectl get pods -n monitoring -l app.kubernetes.io/name=kube-state-metrics

### Setting up Loki via Helm
helm show values grafana/loki > loki-default-values.yaml

### Create loki-values.yaml
nano loki-values.yaml
helm install loki grafana/loki \
--namespace monitoring \
--values loki-values.yaml

### Installing Fluent Bit
helm show values grafana/fluent-bit > fluent-default-values.yaml

### Create fluentbit-values.yaml
nano fluentbit-values.yaml
helm install fluent-bit grafana/fluent-bit \
--namespace monitoring \
--values fluentbit-values.yaml

### Checking the services
microk8s.kubectl get pods -n monitoring
microk8s.kubectl get svc -n monitoring

### Checking the availability of Loki
microk8s.kubectl port-forward svc/loki 3100:3100 -n monitoring
curl http://localhost:3100/ready
