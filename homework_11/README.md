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
nano service.yaml
cd ../

mkdir monitoring
cd monitoring
nano namespace.yaml

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
nano deployment.yaml
nano service.yaml
nano node-exporter.yaml
nano kube-state-metrics.yaml
cd ../

mkdir grafana
cd grafana
nano deployment.yaml
nano service.yaml
cd ../../

#### Create folders
sudo mkdir -p /mnt/data  
sudo chmod 777 /mnt/data

sudo mkdir -p /mnt/data/loki  
sudo chmod -R 777 /mnt/data/loki

## Busybox
#### Create namespace
#### Create PersistentVolume
#### Create PersistentVolumeClaim
#### Create ConfigMap
#### Create Deployment
#### Create Service
#### Create HorizontalPodAutoscaler
microk8s.kubectl apply -f busybox/namespace.yaml
microk8s.kubectl apply -f busybox/pv.yaml
microk8s.kubectl apply -f busybox/pvc.yaml -n busybox-namespace
microk8s.kubectl apply -f busybox/configmap.yaml -n busybox-namespace
microk8s.kubectl apply -f busybox/deployment.yaml -n busybox-namespace
microk8s.kubectl apply -f busybox/service.yaml -n busybox-namespace
microk8s.kubectl apply -f busybox/hpa.yaml -n busybox-namespace

#### Scale Deployment
microk8s.kubectl scale deployment busybox-deployment --replicas=5 -n busybox-namespace
microk8s.kubectl get deployments -n busybox-namespace
microk8s.kubectl get hpa busybox-hpa -n busybox-namespace

#### Check logs
tail /mnt/data/container.log

```code 
busybox-deployment-7794f595db-lhqrq Tue Apr 1 21:41:18 UTC 2025
busybox-deployment-7794f595db-phdqf Tue Apr 1 21:41:18 UTC 2025
busybox-deployment-7794f595db-5xxbv Tue Apr 1 21:41:18 UTC 2025
busybox-deployment-7794f595db-44jsh Tue Apr 1 21:41:19 UTC 2025
busybox-deployment-7794f595db-tcq4v Tue Apr 1 21:41:19 UTC 2025
busybox-deployment-7794f595db-lhqrq Tue Apr 1 21:41:23 UTC 2025
busybox-deployment-7794f595db-phdqf Tue Apr 1 21:41:23 UTC 2025
busybox-deployment-7794f595db-5xxbv Tue Apr 1 21:41:23 UTC 2025
busybox-deployment-7794f595db-44jsh Tue Apr 1 21:41:24 UTC 2025
busybox-deployment-7794f595db-tcq4v Tue Apr 1 21:41:24 UTC 2025
```

## Monitoring
#### Create namespace
microk8s.kubectl apply -f monitoring/namespace.yaml

### Loki
#### Create PersistentVolume
#### Create PersistentVolumeClaim
#### Create ConfigMap
#### Create Deployment
#### Create Service
microk8s.kubectl apply -f monitoring/loki/pv.yaml
microk8s.kubectl apply -f monitoring/loki/pvc.yaml
microk8s.kubectl apply -f monitoring/loki/configmap.yaml
microk8s.kubectl apply -f monitoring/loki/deployment.yaml
microk8s.kubectl apply -f monitoring/loki/service.yaml

### Fluentbit
#### Create ConfigMap
#### Create DaemonSet
microk8s.kubectl apply -f monitoring/fluentbit/configmap.yaml
microk8s.kubectl apply -f monitoring/fluentbit/daemonset.yaml

### Grafana
#### Create Deployment
#### Create Service
microk8s.kubectl apply -f monitoring/grafana/deployment.yaml
microk8s.kubectl apply -f monitoring/grafana/service.yaml

### Prometheus
#### Create Deployment
#### Create Service
#### Node Exporter
#### Create Kube State Metrics
microk8s.kubectl apply -f monitoring/prometheus/deployment.yaml
microk8s.kubectl apply -f monitoring/prometheus/service.yaml
microk8s.kubectl apply -f monitoring/prometheus/node-exporter.yaml
microk8s.kubectl apply -f monitoring/prometheus/kube-state-metrics.yaml

#### Check all
microk8s.kubectl get all --all-namespaces