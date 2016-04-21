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
#####network services
######config ip
```
ip -4 a  //only show ipv4
ip -4 a s eth0   //s=show
```
add
```
ip addr add 172.16.1.2/16 dev eth0
```
192.188.1.1 (first3 :network lastone:host)
######NetworkManager
```
systemctl start NetworkManager  //case sensitive
```
```
nmcli connection show
nmcli connection show eth0
nmcli connection add con-name name ifname eth0 type ethernet ip4 192.168.0.22 gw4 192.168.0.1
nmcli con down name
nmcli con up name
```
######working with network config
```
systemctl status network
```
```
cd /etc/sysconfig/network-scripts
cp ifconfig-eth0 new
```
edit:
```
IPADDR=
NETMASK=
NM_CONTROLLED=
```
```
nmcli con show
```
```
ifdown eth0
```
#####routing
######display route table
```
ip route show  //same as above
ip r 
```
```
route
netstat -r
```
######add route
```
ip r  //then add a route based on existing ip
ip route add default via 192.168.56.1
```
######enable route
```
vim /etc/sysctl.conf
```
edit
```
net.ipv4.ip_forward=1
```
reboot:
```
sysctl -p
```

######NAT
```
iptables -L
iptables -t nat -L
```
```
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
```
#####firewalld
######intro
```
firewall-cmd --state
systemctl start firewalld.service
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones
firewall-cmd --get-zones
```
add, remove zones
```
firewall-cmd --permanent --zone=public --remove-interface=enp0s3
firewall-cmd --permanent --zone=public --remove-interface=enp0s8
firewall-cmd --permanent --zone=external --add-interface=enp0s3
firewall-cmd --permanent --zone=internal --add-interface=enp0s8
systemctl restart firewalld.service
firewall-cmd --get-default-zone=external
```
######working with service
```
firewall-cmd --list-all
firewall-cmd --list-all --zone=internal
```
remove service
```
firewall-cmd --permanent --add-service=ssh
firewall-cmd --permanent --remove-service=ssh
firewall-cmd --permanent --remove-service={1,2,3}
firewall-cmd --reload
ls /usr/lib/firewalld/services/
```
add new empty service
```
firewall-cmd --permanent --new-service="new"
```
```
cd /etc/firewalld/services/
```
```
restorecon new.xml
chmod 640 new.xml
vim new.xml
```
edit
```
<service><short>new</short><port protocal="tcp" port=443/></service>
```
then
```
firewall-cmd --permanent --add-service=new
```
######NAT
```
firewall-cmd --permanent --remove-masquerade
```
