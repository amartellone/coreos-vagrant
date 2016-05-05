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

 We need to run the Apache process in the foreground, since our container will stop when the process specified in the docker run command stops. We can do this with a flag -D when starting the apache2 process:

```
> docker run -d -p 80:80 coreos/apache /usr/sbin/apache2ctl -D FOREGROUND

```
where:

-d, Run container in background and print container ID

-p, Publish a container's port to the host 

host port:container port


Note: The container is running in background but the service inside the container is running in foreground.

# Understand the Docker networking basics

In this lab we analyze the basic networking model used by Docker based on Linux Bridge.

The following picture[1] shows as Docker configures the network layer in its default configuration.

![Docker bridge network](https://github.com/amartellone/coreos-vagrant/blob/master/bridge-network-docker.png)

- eth0: host NIC
- docker0: the default virtual bridge that Docker creates when the Docker deamon starts. Docker picks a subnet not in use on the host and assigns a free IP address to the bridge. Docker0 handles all host-containers communications. When Docker starts a container, by default, it creates a virtual interface on the host with a unique name, such as veth220960a, and an address within the same subnet. This new interface will be connected to the eth0 interface on the container itself. In order to allow connections, iptables rules are added, using a DOCKER-named chain. Network address translation (NAT) is used to forward traffic to external hosts, and the host machine must be set up to forward IP packets. Docker0 is used for connecting all containers on the same host to the local network.  

In order to understand in depth how the network lab is configured, the following commands could be useful:

- ifconfig -a
- brctl show : show a list of bridges, and which are the veths attached to
- docker network ls: list all networks
- vim /etc/systemd/network/50-vagrant1.network (the file name could change)
- sudo iptables --list
- route

# Useful commands

- Running containers

```
> docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    
d78248d4e45d        coreos/apache       "/usr/sbin/apache2ctl"   3 seconds ago       Up 2 seconds        0.0.0.0:80->80/tcp 

```

- Log on to a container 

```
> docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    
d78248d4e45d        coreos/apache       "/usr/sbin/apache2ctl"   3 seconds ago       Up 2 seconds        0.0.0.0:80->80/tcp 


> docker exec -it d78248d4e45d bash

where:
  -i, --interactive=false    Keep STDIN open even if not attached
  -t, --tty=false            Allocate a pseudo-TTY (console)
```

- Docker images 
```
> docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
coreos/apache       latest              7aaa39173c60        2 years ago         294.4 MB

```

- Docker start, stop, kill 
```
> docker start d78248d4e45d
> docker stop d78248d4e45d
> docker kill d78248d4e45d

```

- Docker commit: save an image to the local images repository
```
> docker commit d78248d4e45d webapp

> docker images 

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
webapp              latest              364d5388adc6        2 days ago          374.8 MB
coreos/apache       latest              7aaa39173c60        2 years ago         294.4 MB


```
# Install a Wordpress container using MariaDb

For this example will be used docker-compose, that is not installed by default.

* Create the following bash script, and run it.[2] 


```

#!/bin/bash
mkdir -p /opt/bin
curl -L `curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r '.assets[].browser_download_url | select(contains("Linux") and contains("x86_64"))'` > /opt/bin/docker-compose
chmod +x /opt/bin/docker-compose


```

* Create the file docker-compose.yml[3]

```

wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 8080:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: example


```
In order to connect two or more containers the legacy feature (based on bridge) is using linking system that allows you to link multiple containers together and send connection information from one to another.
When containers are linked, information about a source container can be sent to a recipient container. The naming convention is <name or id>:alias. The name or the id must be unique[4].


* Run docker-compose up, visit <ip:8080> and install Wordpress.

# References
[1] http://www.linuxjournal.com/content/concerning-containers-connections-docker-networking

[2] https://gist.github.com/marszall87/ee7c5ea6f6da9f8968dd

[3] https://hub.docker.com/_/wordpress/

[4] https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/


