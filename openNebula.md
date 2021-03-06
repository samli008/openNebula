## OpenNebula has two main components:
```
1. OpenNebula Front-end – This is the management engine that executes the OpenNebula services.
2. OpenNebula Hypervisor Nodes – These are the hypervisors which provide the resources needed by the VMs.
```
## Minimum Recommended configuration
```
Memory 2G
CPU 1CPU 2cores
Disk size 100GB
Network 2nics
```
## install Front-end
```
cat > /etc/yum.repos.d/opennebula.repo << EOF
[opennebula]
name=opennebula
baseurl=file:///root/opennebula
enabled=1
gpgcheck=0
EOF

yum -y install opennebula opennebula-server opennebula-sunstone opennebula-ruby opennebula-gate opennebula-flow

gem install bundler -v 1.17.3
/usr/share/one/install_gems

echo "oneadmin:liyang@008" > /var/lib/one/.one/one_auth

systemctl start opennebula
systemctl enable opennebula
systemctl start opennebula-sunstone
systemctl enable opennebula-sunstone

oneuser show
http://ip:9869 user:oneadmin passwd:liyang@008
```
## install host nodes
```
cat > /etc/yum.repos.d/opennebula.repo << EOF
[opennebula]
name=opennebula
baseurl=file:///root/opennebula
enabled=1
gpgcheck=0
EOF

yum -y install opennebula-node-kvm
systemctl start libvirtd
systemctl enable libvirtd

vi  /etc/libvirt/libvirtd.conf
unix_sock_group = "oneadmin"
unix_sock_rw_perms = "0777"

systemctl restart libvirtd
```
## add host nodes ssh trust from Front-end
```
passwd oneadmin

su - oneadmin
ssh-keygen

scp /var/lib/one/.ssh/*  node1:/var/lib/one/.ssh/
scp /var/lib/one/.ssh/*  node2:/var/lib/one/.ssh/

ssh node1
ssh node2
```
## Install and Configure MySQL database
```
yum -y install mariadb-server mariadb
systemctl enable mariadb
systemctl start mariadb
```
## Setup root password for MariaDB
```
mysql_secure_installation
```
## Create a database and user for OpenNebula
```
mysql -u root -p

CREATE DATABASE opennebula;
GRANT ALL PRIVILEGES ON opennebula.* TO 'oneadmin' IDENTIFIED BY 'StrongPassword';
FLUSH PRIVILEGES;
```
## Configure OpenNebula DB
```
vi /etc/one/oned.conf

#DB = [ BACKEND = "sqlite" ]

DB = [ backend = "mysql",
 server = "localhost",
 port = 0,
 user = "oneadmin",
 passwd = "StrongPassword",
 db_name = "opennebula" ]

mysql -u oneadmin -p
```
## Configure Firewall
```
firewall-cmd --state
firewall-cmd --add-port=9869/tcp --permanent
firewall-cmd --reload
```
