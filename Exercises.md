# Prerequisites

- Run the VM
```
> vagrant up

```

- Log on to the VM
```
> vagrant ssh

```

# Create the first Docker container: Apache

- Run an Apache container
```
> docker run -d -p 80:80 coreos/apache /usr/sbin/apache2ctl -D FOREGROUND

```
where:
-d, Run container in background and print container ID
-p, Publish a container's port(s) to the host
80:80, <host port>:<container port> 

# Useful commands

- Running containers
```
> docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    
d78248d4e45d        coreos/apache       "/usr/sbin/apache2ctl"   3 seconds ago       Up 2 seconds        0.0.0.0:80->80/tcp 

```

- Host-only NIC
```
> ifconfig eth1

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.8.101  netmask 255.255.255.0  broadcast 172.17.8.255
        inet6 fe80::a00:27ff:fe2f:9528  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:2f:95:28  txqueuelen 1000  (Ethernet)
        RX packets 17  bytes 1564 (1.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 29  bytes 2242 (2.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```
