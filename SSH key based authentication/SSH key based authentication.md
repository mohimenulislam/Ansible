
## SSH key based authentication
SSH key authentication is also more convenient than password authentication. 

The keys connect users and processes to a server by initiating authentication 
and granting access automatically, so users don't have to remember or enter their password 
for each and every system. 


> [!NOTE]
> - Controlnode shoud be linux
> - If an command executed in managed node from controlnode, if the managed node wil be linux it communicate/login through `ssh`, and for windows it communicate through `winrm`.
> -  To install a package in managed node from controlnode it must have `authentication`, `authorization`. And also authenticate by a user and user must be sudo privileged.
> -  Mostly root login denied for security purpose.
> -  Reguler user can't install any package, can't write any configuration file.
 

#### SSH server
- `Package name`:- openssh-server ... rpm
- `Config File`:- /etc/ssh/sshd_config
- `Default Port`: 22/tcp
- `Service`: sshd

#### SSH server Client
- `Package name`:- openssh-clients
- `Config File`:- /etc/ssh/ssh_config


#### SSH Server installation 
```bash
yum install openssh-server -y
systemctl start sshd
systemctl enable sshd
systemctl status sshd

# Check ssh service is allow in firewall or no
firewall-cmd --list-all

# if no allow
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload

```
#### Importent Points
-  `Port` - Bydefault 22
- `Listen` - If have multiple IP, which IP will listen from.
- ` PermitRootLogin`:- Bydefault `prohibit-password`
  - `prohibit-password`- Root Login deny ,but login by key based authentication. 
  - `yes` - SSH not allow
  - `no` -  SSH allow 
- `AllowUsers` - AllowUser means, speacific user allow & all user is deny. If use `AllowUsers`, don't use `Denyusers`.
  - if `user1` is AllowUsers. Then all others user is denied
- `Denyusers` - If `user2` is defined as `Denyusers`, then user2 can't SSH to system.
- `AllowGroup` - The primary function of AllowGroups is to restrict SSH access to your server based on group membership. Same as `AllowUsers`.
- `DenyGroup` -  Unlike DenyUsers which blocks individual users, `DenyGroups` applies to entire groups.


Generate ssh key Pair (Public and Private key) 
```bash
ssh-keygen 
```

Copy the public key to server ( ~/.ssh/authorized_keys) 
```bash
ssh-copy-id root@host1  # or
ssh-copy-id root@192.168.10.131
```

Now, we login as `alex` user 
