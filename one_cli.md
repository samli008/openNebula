## setup datastore and modify SAFE_DIRS="/var/lib/one"
```
onedatastore list
onedatastore show 104
onedatastore update 104
```
## add image
```
cat > /var/lib/one/image.conf << EOF
NAME = Centos7.6
PERSISTENT = NO
PATH = /var/lib/one/c76.img
TYPE = OS
DRIVER = qcow2
DESCRIPTION = "made in liyang"
EOF

oneimage list
oneimage create image.conf -d 104
oneimage show 7
```
## vm operation
```
onevm list
onevm disk-saveas test1 0 c78
onevm recover --delete 110
```
## vnet operation
```
onevnet list
```
## host operation
```
onehost list
```
## logs
```
cd /var/log/one
vi sched.log
vi oned.log
```
