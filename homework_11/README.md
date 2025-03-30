## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_11
cd homework_11

## Create files
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
nano daemon.yaml
cd ../

mkdir prometheus
cd prometheus
nano configmap.yaml
nano deployment.yaml
nano service.yaml
nano node-exporter
cd ../

mkdir grafana
cd grafana
nano deployment.yaml
nano service.yaml
cd ../../