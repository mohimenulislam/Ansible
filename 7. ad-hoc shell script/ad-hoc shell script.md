
## ad-hoc shell script

```
[webserver]
host1

[dbserver]
host2

[webdb:children]
webserver
dbserver
```
If we add an single host to webdb
```
[webserver]
host1

[dbserver]
host2

[webdb:children]
webserver
dbserver
[webdb]
host3
```

```
ansible all --list-hosts
```

Suppose the `useradd` command itself is a shell script and all command stored at:
```
env | grep -i path

output:
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin        
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/0/bus
```

To create a bash file, start with
```
touch ad-hok.sh

#!/bin/bash
```

```
ls -l ad-hok.sh
chmod +x ad-hok.sh
```

#### Make ad-hoc command to run http service 
```
#!/bin/bash

ansible host1 -m yum -a "name=httpd state=latest"
ansible host1 -m service -a "name=httpd state=started enabled=yes"
ansible host1 -m firewalld -a "service=httpd state=enabled permanent=yes immediate=yes"
```
