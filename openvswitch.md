## setup openvswitch
```
ovs-vsctl add-br br100

ovs-vsctl add-port br100 em2

ovs-vsctl set port em2 trunks=6,8

ovs-vsctl clear port em2 trunks

ovs-vsctl add-bond ontap-internal bond-br p2p1 p2p2 bond_mode=balance-slb lacp=active other_config:lacp-time=fast

ovs-vsctl show
```
