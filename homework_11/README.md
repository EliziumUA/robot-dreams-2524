#### Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_11
cd homework_11

#### Create files
mkdir busybox
cd busybox
nano namespace.yaml
nano pv.yaml
nano pvc.yaml
nano configmap.yaml
nano deployment.yaml
nano hpa.yaml
cd ../

mkdir monitoring
nano namespace.yaml
cd monitoring

mkdir loki
cd loki
nano pv.yaml
nano pvc.yaml
nano configmap.yaml
nano deployment.yaml
nano service.yaml
cd ../

mkdir fluentbit
cd fluentbit
nano configmap.yaml
nano daemonset.yaml
cd ../

mkdir prometheus
cd prometheus
nano configmap.yaml
nano deployment.yaml
nano service.yaml
nano daemonset.yaml
cd ../

mkdir grafana
cd grafana
nano deployment.yaml
nano service.yaml
cd ../../

## Busybox
#### Create namespace
microk8s.kubectl apply -f busybox/namespace.yaml
microk8s.kubectl get namespaces

#### Create PersistentVolume
microk8s.kubectl apply -f busybox/pv.yaml
microk8s.kubectl get pv -n my-busybox

#### Create PersistentVolumeClaim
microk8s.kubectl apply -f busybox/pvc.yaml -n my-busybox
microk8s.kubectl get pvc -n my-busybox

#### Create ConfigMap
microk8s.kubectl apply -f busybox/configmap.yaml -n my-busybox
microk8s.kubectl get configmap -n my-busybox

#### Create Deployment
microk8s.kubectl apply -f busybox/deployment.yaml -n my-busybox
microk8s.kubectl get deployments -n my-busybox

#### Create HorizontalPodAutoscaler
microk8s.kubectl apply -f busybox/hpa.yaml -n my-busybox
microk8s.kubectl get hpa -n my-busybox

## Monitoring
#### Create namespace
microk8s.kubectl apply -f monitoring/namespace.yaml
microk8s.kubectl get namespaces

### Loki
#### Create PersistentVolume
microk8s.kubectl apply -f monitoring/loki/pv.yaml
microk8s.kubectl get pv -n my-monitoring

#### Create PersistentVolumeClaim
microk8s.kubectl apply -f monitoring/loki/pvc.yaml
microk8s.kubectl get pvc -n my-monitoring

#### Create ConfigMap
microk8s.kubectl apply -f monitoring/loki/configmap.yaml
microk8s.kubectl get configmap -n my-monitoring

#### Create Deployment
microk8s.kubectl apply -f monitoring/loki/deployment.yaml
microk8s.kubectl get deployments -n my-monitoring

#### Create Service
microk8s.kubectl apply -f monitoring/loki/service.yaml
microk8s.kubectl get services -n my-monitoring

### Fluentbit
#### Create ConfigMap
microk8s.kubectl apply -f monitoring/fluentbit/configmap.yaml
microk8s.kubectl get configmap -n my-monitoring

#### Create DaemonSet
microk8s.kubectl apply -f monitoring/fluentbit/daemonset.yaml
microk8s.kubectl get daemonset -n my-monitoring

### Prometheus
#### Create ConfigMap
microk8s.kubectl apply -f monitoring/prometheus/configmap.yaml
microk8s.kubectl get configmap -n my-monitoring

#### Create Deployment
microk8s.kubectl apply -f monitoring/prometheus/deployment.yaml
microk8s.kubectl get deployments -n my-monitoring

#### Create Service
microk8s.kubectl apply -f monitoring/prometheus/service.yaml
microk8s.kubectl get services -n my-monitoring

#### Create DaemonSet
microk8s.kubectl apply -f monitoring/prometheus/daemonset.yaml
microk8s.kubectl get daemonset -n my-monitoring

### Grafana
#### Create Deployment
microk8s.kubectl apply -f monitoring/grafana/deployment.yaml
microk8s.kubectl get deployments -n my-monitoring

#### Create Service
microk8s.kubectl apply -f monitoring/grafana/service.yaml
microk8s.kubectl get services -n my-monitoring
