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

/usr/share/one/install_gems

echo "oneadmin:liyang@008" > /var/lib/one/.one/one_auth

systemctl start opennebula
systemctl enable opennebula
systemctl start opennebula-sunstone
systemctl enable opennebula-sunstone

http://ip:9869
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
