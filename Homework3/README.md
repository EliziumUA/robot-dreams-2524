## Create directory
mkdir Storage
cd Storage
mkdir Homework3

## Create files
nano Dockerfile
nano index.html

## Build image
sudo docker build -t my-nginx .

## Start container
sudo docker run -d -p 8080:80 my-nginx

## Run curl
curl http://localhost:8080

## Show running containers
sudo docker ps

```code 
CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                                     NAMES
41dccfabb8ee   my-nginx   "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   silly_stonebraker
```

## Stop container
sudo docker stop 41dccfabb8ee

## Show all containers
sudo docker ps -a

```code 
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                      PORTS     NAMES
41dccfabb8ee   my-nginx      "/docker-entrypoint.…"   31 seconds ago   Exited (0) 11 seconds ago             silly_stonebraker busy_rhodes
```

## Delete container
sudo docker rm 41dccfabb8ee

```code 
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                      PORTS     NAMES
```
