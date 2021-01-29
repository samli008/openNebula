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
```
## add host nodes ssh trust from Front-end
```
su oneadmin
ssh-keygen -t rsa
cat /var/lib/one/.ssh/id_rsa.pub >> /var/lib/one/.ssh/authorized_keys
chmod 644 /var/lib/one/.ssh/authorized_keys
chmod 755 /var/lib/one/.ssh/

scp /var/lib/one/.ssh/*  root@node1:/var/lib/one/.ssh/
scp /var/lib/one/.ssh/*  root@node2:/var/lib/one/.ssh/
```
