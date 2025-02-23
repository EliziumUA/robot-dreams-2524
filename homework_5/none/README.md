## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_5
cd homework_5
mkdir none
cd none

## None network
docker run -d --name container4 --network none busybox sleep 3600

docker inspect container4 | grep -i "network"

```code
"NetworkMode": "none",
"NetworkSettings": {
    "Networks": {
            "NetworkID": "4aac27574d385dcb963e17fa8f462fe8a0446357b2f653129b5cc44cfbe08ab3",
```

docker exec -it container4 ip a

```code
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
```

docker stop 9a41f7a9adfc
docker rm 9a41f7a9adfc
