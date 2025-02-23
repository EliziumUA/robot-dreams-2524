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
docker stop container1
docker stop container2

## Delete containers
docker rm container1
docker rm container2

## Delete volume
docker volume rm my_volume
