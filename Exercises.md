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

