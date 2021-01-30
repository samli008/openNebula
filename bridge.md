## setup host bridge network with vlan
```
read -p "please input vlan interface: " int
read -p "please input add vlan id: " vlan

cat > /etc/sysconfig/network-scripts/ifcfg-$int.$vlan << EOF
DEVICE=$int.$vlan
ONBOOT=yes
BOOTPROTO=none
VLAN=yes
BRIDGE=br$vlan
EOF

cat > /etc/sysconfig/network-scripts/ifcfg-br$vlan << EOF
DEVICE=br$vlan
TYPE=Bridge
ONBOOT=yes
BOOTPROTO=none
VLAN=yes
EOF

ifup br$vlan
ifup $int.$vlan
brctl show |grep $vlan
```
