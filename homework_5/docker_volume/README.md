# Docker Volume

## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_5
cd homework_5
mkdir docker_volume
cd docker_volume

## Create volume
docker volume create my_volume

## Run containers
docker run -d --name container1 -v my_volume:/data busybox sleep 3600
docker run -d --name container2 -v my_volume:/data busybox sleep 3600

## Edit shared file
docker exec -ti container1 vi /data/payload.txt
docker exec -ti container2 cat /data/payload.txt

## Stop containers
docker stop be9a3988ae98
docker stop c2d11bc6c763

## Delete containers
docker rm be9a3988ae98
docker rm c2d11bc6c763

## Delete volume
docker volume rm my_volume
