## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_5
cd homework_5
mkdir bridge
cd bridge

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
            "NetworkID": "514574bdfb9f9c385cfc4fdf5c5891bd8a1bc3650f6392f68ef7a1b9ec7b21d8",
```

```code 
"NetworkMode": "my_bridge_network",
"NetworkSettings": {
    "Networks": {
        "my_bridge_network": {
            "NetworkID": "514574bdfb9f9c385cfc4fdf5c5891bd8a1bc3650f6392f68ef7a1b9ec7b21d8",
```

docker exec -it container1 ip a
docker exec -it container2 ip a

```code 
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
176: eth0@if177: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue 
    link/ether 02:42:ac:12:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.2/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```

```code 
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
178: eth0@if179: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue 
    link/ether 02:42:ac:12:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.3/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever
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

docker stop e2043496e515
docker stop a15b42272686

docker rm e2043496e515
docker rm a15b42272686

docker network rm my_bridge_network
