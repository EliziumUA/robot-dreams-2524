## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_4
cd homework_4
mkdir tags
cd tags
mkdir 0_1
mkdir 0_2

## DockerHub login
docker login -u eliziumua

## Unoptimized image
cd 0_1
nano Dockerfile
nano index.html
docker build -t my-nginx-image:0.1 .
docker tag my-nginx-image:0.1 eliziumua/my-nginx-image:0.1
docker push eliziumua/my-nginx-image:0.1
docker pull eliziumua/my-nginx-image:0.1
docker run -d -p 8080:80 eliziumua/my-nginx-image:0.1
docker ps
```code 
CONTAINER ID   IMAGE                          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
b41d7ebe811b   eliziumua/my-nginx-image:0.1   "nginx -g 'daemon of…"   27 seconds ago   Up 26 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   cranky_khorana
```
docker stop b41d7ebe811b
docker rm b41d7ebe811b
cd ../

## Optimized image
cd 0_2
nano Dockerfile
nano index.html
docker build -t my-nginx-image:0.2 .
docker tag my-nginx-image:0.2 eliziumua/my-nginx-image:0.2
docker push eliziumua/my-nginx-image:0.2
docker pull eliziumua/my-nginx-image:0.2
docker run -d -p 8080:80 eliziumua/my-nginx-image:0.2
docker ps
```code 
CONTAINER ID   IMAGE                          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
95d285170cae   eliziumua/my-nginx-image:0.2   "/docker-entrypoint.…"   16 seconds ago   Up 15 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   xenodochial_gates
```
docker stop 95d285170cae
docker rm 95d285170cae

## Result
```code 
root@mainserver:~/roman_podobnyi/homework_4/tags# docker images
REPOSITORY                 TAG       IMAGE ID       CREATED          SIZE
eliziumua/my-nginx-image   0.2       a64afbb994f9   30 minutes ago   57.3MB
eliziumua/my-nginx-image   0.1       806a5a4990a7   31 minutes ago   212MB
```
