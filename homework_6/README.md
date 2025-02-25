## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_6
cd homework_6

## Create .env
nano .env
```code
GF_SECURITY_ADMIN_PASSWORD=admin
GF_DASHBOARD_DEFAULT_HOME_DASHBOARD_PATH=/etc/grafana/dashboards/default-dashboard.json
GF_SERVER_ROOT_URL=http://localhost:3000
```

## Create Dockerfile
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

## Create network
docker network create --driver bridge my_network
docker network ls

```code
NETWORK ID     NAME         DRIVER    SCOPE
5cc0f2b30ed7   bridge       bridge    local
d1b6a6789515   host         host      local
09ec118cf16c   my_network   bridge    local
4aac27574d38   none         null      local
```

## Build image
docker build -t fluentd-loki .

## Run container Fluentd
docker run -d --name fluentd \
--network my_network \
-v $(pwd)/fluentd.conf:/fluentd/etc/fluent.conf \
-p 24224:24224 -p 24224:24224/udp \
fluentd-loki

docker ps
```code
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                                                                                    NAMES
1c41e35ae774   fluentd-loki   "tini -- /bin/entryp…"   5 seconds ago   Up 4 seconds   5140/tcp, 0.0.0.0:24224->24224/tcp, 0.0.0.0:24224->24224/udp, :::24224->24224/tcp, :::24224->24224/udp   fluentd
```

## Run container with log generation
docker run -d --name log_container_fluentd \
--network my_network \
--log-driver=fluentd --log-opt fluentd-address=localhost:24224 \
busybox sh -c 'while true; do echo "Fluentd test log: $(date)"; sleep 2; done'

docker ps
```code
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                                                                                    NAMES
015960223cf4   busybox        "sh -c 'while true; …"   6 seconds ago    Up 5 seconds                                                                                                             log_container_fluentd
1c41e35ae774   fluentd-loki   "tini -- /bin/entryp…"   25 seconds ago   Up 24 seconds   5140/tcp, 0.0.0.0:24224->24224/tcp, 0.0.0.0:24224->24224/udp, :::24224->24224/tcp, :::24224->24224/udp   fluentd
```

## Run container Loki
docker run -d --name loki --network my_network -p 3100:3100 grafana/loki:latest

docker ps
```code
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                                                                                    NAMES
f252fe393ed6   grafana/loki:latest   "/usr/bin/loki -conf…"   5 seconds ago    Up 3 seconds    0.0.0.0:3100->3100/tcp, :::3100->3100/tcp                                                                loki
015960223cf4   busybox               "sh -c 'while true; …"   26 seconds ago   Up 25 seconds                                                                                                            log_container_fluentd
1c41e35ae774   fluentd-loki          "tini -- /bin/entryp…"   45 seconds ago   Up 44 seconds   5140/tcp, 0.0.0.0:24224->24224/tcp, 0.0.0.0:24224->24224/udp, :::24224->24224/tcp, :::24224->24224/udp   fluentd
```

## Run container Grafana
docker run -d --name grafana \
--network my_network -p 3000:3000 \
-e GF_SECURITY_ADMIN_PASSWORD=${GF_SECURITY_ADMIN_PASSWORD} \
-e GF_DASHBOARD_DEFAULT_HOME_DASHBOARD_PATH=${GF_DASHBOARD_DEFAULT_HOME_DASHBOARD_PATH} \
-e GF_SERVER_ROOT_URL=${GF_SERVER_ROOT_URL} -v $(pwd)/provisioning:/etc/grafana/provisioning grafana/grafana

docker ps
```code
CONTAINER ID   IMAGE                 COMMAND                  CREATED              STATUS              PORTS                                                                                                    NAMES
3f9bb973b780   grafana/grafana       "/run.sh"                4 seconds ago        Up 3 seconds        0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                                grafana
f252fe393ed6   grafana/loki:latest   "/usr/bin/loki -conf…"   30 seconds ago       Up 29 seconds       0.0.0.0:3100->3100/tcp, :::3100->3100/tcp                                                                loki
015960223cf4   busybox               "sh -c 'while true; …"   51 seconds ago       Up 50 seconds                                                                                                                log_container_fluentd
1c41e35ae774   fluentd-loki          "tini -- /bin/entryp…"   About a minute ago   Up About a minute   5140/tcp, 0.0.0.0:24224->24224/tcp, 0.0.0.0:24224->24224/udp, :::24224->24224/tcp, :::24224->24224/udp   fluentd
```

## Stop containers
docker stop log_container_fluentd fluentd loki grafana

## Delete container
docker rm log_container_fluentd fluentd loki grafana

## Delete network
docker network rm my_network
