## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_7
cd homework_7

## Create .env
nano .env
```code
GF_SECURITY_ADMIN_PASSWORD=admin
GF_DASHBOARD_DEFAULT_HOME_DASHBOARD_PATH=/etc/grafana/dashboards/default-dashboard.json
GF_SERVER_ROOT_URL=http://localhost
```

## Create Dockerfile
mkdir fluentd-loki
nano Dockerfile
```code
FROM fluent/fluentd:v1.16-debian
USER root
RUN gem install fluent-plugin-loki
USER fluent
```

## Create fluentd.conf
nano fluentd.conf
```code
<source>
  @type forward
</source>

<match **>
  @type loki
  endpoint_url "http://loki:3100"
  labels {"job":"docker-logs"}
</match>
```

## Create datasources.yaml
mkdir -p provisioning/datasources
nano datasources.yaml
```code
apiVersion: 1

datasources:
  - name: Loki
    type: loki
    access: proxy
    url: http://loki:3100
    isDefault: true
```

## Build containers
docker compose build

## Run containers
docker compose up -d

## Stop containers
docker compose down
