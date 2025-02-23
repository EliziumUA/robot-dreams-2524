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

## None network
docker run -d --name container3 --network none busybox sleep 3600
docker inspect container3 | grep -i "network"

```code
"NetworkMode": "none",
"NetworkSettings": {
    "Networks": {
            "NetworkID": "4aac27574d385dcb963e17fa8f462fe8a0446357b2f653129b5cc44cfbe08ab3",
```

docker exec -it container3 ip a

```code
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
```

docker stop 2079e61061c2
docker rm 2079e61061c2

## Host network
docker run -d --name container4 --network host busybox sleep 3600
docker inspect container4 | grep -i "network"

```code
"NetworkMode": "host",
"NetworkSettings": {
    "Networks": {
            "NetworkID": "d1b6a6789515bb03586edb8679f5a5d3941f6b93bb4903b1a85fe7fbbd6e087a",
```

docker exec -it container4 ip a

```code
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
.........
.........
.........
145: calibdb5b920573@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
```

docker stop d556fdd39de2
docker rm d556fdd39de2

## Macvlan network
