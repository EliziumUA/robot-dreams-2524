# Host network

## Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_5
cd homework_5
mkdir host
cd host

## Run container
docker run -d --name container3 --network host busybox sleep 3600

## Inspect container
docker inspect container3 | grep -i "network"

```code
"NetworkMode": "host",
"NetworkSettings": {
    "Networks": {
            "NetworkID": "d1b6a6789515bb03586edb8679f5a5d3941f6b93bb4903b1a85fe7fbbd6e087a",
```

## Check container networks
docker exec -it container3 ip a

```code
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq qlen 1000
    link/ether fa:16:3e:86:76:cf brd ff:ff:ff:ff:ff:ff
    inet 78.27.236.215/24 brd 78.27.236.255 scope global dynamic enp3s0
       valid_lft 35811sec preferred_lft 35811sec
    inet6 fe80::f816:3eff:fe86:76cf/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue 
    link/ether 02:42:a2:c1:25:ba brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:a2ff:fec1:25ba/64 scope link 
       valid_lft forever preferred_lft forever
11: vxlan.calico: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether 66:b2:3a:72:ca:79 brd ff:ff:ff:ff:ff:ff
    inet 10.1.58.128/32 scope global vxlan.calico
       valid_lft forever preferred_lft forever
    inet6 fe80::64b2:3aff:fe72:ca79/64 scope link 
       valid_lft forever preferred_lft forever
191: cali3ef116d62da@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
192: cali832dd1728c6@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
193: cali76bb211800e@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
194: cali9af3d00227c@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
195: calie03ac035fd5@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
196: calibdb5b920573@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
197: cali97b04b5d782@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
198: calib67777a08e2@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
199: calicd76743ca27@enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue qlen 1000
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link 
       valid_lft forever preferred_lft forever
```

## Stop container
docker stop a17844dfd85d

## Delete container
docker rm a17844dfd85d
