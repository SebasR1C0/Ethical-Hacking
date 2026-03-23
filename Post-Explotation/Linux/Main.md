# Tools
[LinPeas](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS) and [LinEnum](https://github.com/rebootuser/LinEnum)

[Lynis](https://github.com/cisofy/lynis)

# User Information
```
whoami
id
hostname
env
history
cat /etc/shadow
```
# Privileges
```
# Privileges
sudo -l

# SUID
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
find / -uid 0 -perm -6000 -type f 2>/dev/null

# Path Abuse
echo $PATH

# Enumeration of application 
cat /etc/shells
```

# System Information
```
# OS system
cat /etc/os-release

# Kernel
uname -a

# Initial Procces
/etc/fstab

# CPU info
lscpu
```

# Services
```
# Port
netstat -tulnp

# Cronjob
ls -la /etc/cron.daily/

# Proccesses
ps aux

# Installed app
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list

# Outdated version
sudo -V
```

# Common Files
- .*
- *_history or _hist
- *.bak
- *.conf -o -name .config
- *.py -o -name .sh
Code
```
find / -type f \( -iname *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```

# Network
```
# Internal connection
ip a
cat /etc/hosts

# Devices
lsblk
df -h

# DNS connection
arp -a

# Printer status
lpstat
```

# Uncommon attacks
- Application or private information in path Documents, Downloads and Desktop
- Path Abuse
- Wildcard Abuse 
- Escaping Restricted Shells (ssh htb-user@10.129.205.109 bash)
