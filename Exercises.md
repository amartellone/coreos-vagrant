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

-p, Publish a container's port to the host 

host port:container port

# Understand the Docker networking basics

In this lab we analyze the basic model based on Linux Bridge.


# Useful commands

- Running containers
- 
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
