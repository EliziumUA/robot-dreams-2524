# Macvlan network

### Create directory
mkdir roman_podobnyi
cd roman_podobnyi
mkdir homework_5
cd homework_5
mkdir macvlan
cd macvlan

## Static IP
### Display network interfaces
ip a

```code 
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether fa:16:3e:86:76:cf brd ff:ff:ff:ff:ff:ff
    inet 78.27.236.215/24 metric 100 brd 78.27.236.255 scope global dynamic enp3s0
       valid_lft 27966sec preferred_lft 27966sec
    inet6 fe80::f816:3eff:fe86:76cf/64 scope link 
       valid_lft forever preferred_lft forever
```

### Create network
docker network create -d macvlan \
--subnet=192.168.1.0/24 \
--gateway=192.168.1.1 \
-o parent=enp3s0 my_macvlan_network

### Run container
docker run -d --name container5 --network my_macvlan_network --ip 192.168.1.100 busybox sleep 3600

### Inspect container
docker inspect container5 | grep "IPAddress"

```code
"SecondaryIPAddresses": null,
"IPAddress": "",
        "IPAddress": "192.168.1.100",
```

### Stop container
docker stop container5

### Delete container
docker rm container5

### Delete network
docker network rm my_macvlan_network

## DHCP
### Display network interfaces
ip a

```code 
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether fa:16:3e:86:76:cf brd ff:ff:ff:ff:ff:ff
    inet 78.27.236.215/24 metric 100 brd 78.27.236.255 scope global dynamic enp3s0
       valid_lft 27966sec preferred_lft 27966sec
    inet6 fe80::f816:3eff:fe86:76cf/64 scope link 
       valid_lft forever preferred_lft forever
```

### Create network
docker network create -d macvlan \
--subnet=192.168.1.0/24 \
--gateway=192.168.1.1 \
-o parent=enp3s0 \
-o macvlan_mode=bridge my_macvlan_network

### Run container
docker run -d --name container6 --net my_macvlan_network busybox sleep 3600

### Display container network interfaces
docker exec -it container6 ip a

```code
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
226: eth0@if2: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue 
    link/ether 02:42:c0:a8:01:02 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.2/24 brd 192.168.1.255 scope global eth0
       valid_lft forever preferred_lft forever
```

### Run container
docker run -d --name container7 --net my_macvlan_network busybox sleep 3600

### Display container network interfaces
docker exec -it container7 ip a

```code
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
227: eth0@if2: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue 
    link/ether 02:42:c0:a8:01:03 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.3/24 brd 192.168.1.255 scope global eth0
       valid_lft forever preferred_lft forever
```

### Stop containers
docker stop container6
docker stop container7

### Delete containers
docker rm container6
docker rm container7

### Delete network
docker network rm my_macvlan_network
