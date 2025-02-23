## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_5
cd homework_5

## Bridge network
docker network create my_bridge_network
docker run -d --name container1 --network my_bridge_network busybox sleep 3600
docker run -d --name container2 --network my_bridge_network busybox sleep 3600

docker inspect container1 | grep -i "network"
docker inspect container2 | grep -i "network"

```code 
"NetworkMode": "my_bridge_network",
"NetworkSettings": {
    "Networks": {
        "my_bridge_network": {
            "NetworkID": "62406f07cc8d258b9208e6e74a03e7ea3c4e7b0f67666e75df40e2fb481dd97a",
```

docker exec -it container1 ping -c 3 container2
docker exec -it container2 ping -c 3 container1

```code 
PING container2 (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.349 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.092 ms
64 bytes from 172.18.0.3: seq=2 ttl=64 time=0.107 ms
--- container2 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.092/0.182/0.349 ms
```

```code 
PING container1 (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.101 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.078 ms
64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.104 ms
--- container1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.078/0.094/0.104 ms
```

docker stop cd373e6010ed
docker stop 1122813be50a
docker rm cd373e6010ed
docker rm 1122813be50a
docker network rm my_bridge_network

## Host network
## None network
## Macvlan network
