#### pslfcslinuxnetwork
#####intro
######ip ifconfig
```
ip r
ip n
```
add namespace:
```
sudo ip netns add newname
```
#####hostname
######view
```
hostnamectl
```
######config
change new name
```
hostname newname //transient
```
change host file
```
hostnamectl set-hostname newname.example.com
ping new
```
set pretty name
```
hostnamectl set-hostname newname'1.example.com
```
see the pretty name
```
cat /etc/machine-info
```
######local hosts file
edit /etc/hosts ,append aliases
```
192.168.1.1 new.example.com alias1 alias2
```
######DNS name resolution
```
getent hosts
grep hosts /etc/nsswitch.conf
cat /etc/resolv.conf
```
install dig
```
sudo yum install -y bind-utils
```
```
dig www.xx.com
dig +short www.xx.com @8.8.8.8
```
others:use avahi

#####NTP
######using date and hwclock
set current system time
```
hwclock --systohc  systime->hardware
```
set date with format
```
date --set="20160101 11:01"
```
set time from hardware
```
hwclock --hctosys
```
######timedatectl
change time and hwclock.
```
timedatectl set-time "2016-01-01 12:11:11"   //this will not be allowed
timedatectl set-ntp false  //unlock
```
######chronyd
```
yum install -y chrony
```
```
vim /etc/chrony.conf
```
```
systemctl start chronyd
```
```
chronyc tracking
chronyc sources
```
######ntpd
```
yum -y install ntp
```
```
vim /etc/ntp.conf
```
```
systemctl start ntpd
```

