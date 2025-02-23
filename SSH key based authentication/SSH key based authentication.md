
## SSH key based authentication
SSH key authentication is also more convenient than password authentication. 

The keys connect users and processes to a server by initiating authentication 
and granting access automatically, so users don't have to remember or enter their password 
for each and every system. 

#### SSH Key Pair
- `Public Key` >> Lock
- `Private Key` >> Key

  
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
  - `yes` - SSH allow
  - `no` -  SSH not allow 
- `AllowUsers` - AllowUser means, speacific user allow & all user is deny. If use `AllowUsers`, don't use `Denyusers`.
  - if `user1` is AllowUsers. Then all others user is denied
- `Denyusers` - If `user2` is defined as `Denyusers`, then user2 can't SSH to system.
- `AllowGroup` - The primary function of AllowGroups is to restrict SSH access to your server based on group membership. Same as `AllowUsers`.
- `DenyGroup` -  Unlike DenyUsers which blocks individual users, `DenyGroups` applies to entire groups.
- `PubkeyAuthentication` - By default PubkeyAuthentication is enabled. 
  -  `yes` -  PubkeyAuthentication will work.
  -  `no` - PubkeyAuthentication will not work.


#### Generate ssh key Pair (Public and Private key) 
```bash
ssh-keygen

# A key pair will generate under /root/.ssh
  # id_rsa     >> private key
  # id_rsa.pub >> public key
```

#### Copy the public key to host1 ( ~/.ssh/authorized_keys) 
```bash
ssh-copy-id root@host1  # or
ssh-copy-id root@192.168.10.131

# this command copy public key(id_rsa.pub) to host machine and save as .ssh/authorized_keys
```

#### For Security purpose we will move private key(id_rsa) in another location 
```bash
mv .ssh/id_rsa /opt/           # moved private key to /opt
mv /opt/id_rsa key             # rename id_rsa to key
ssh -i /opt/key root@host1     # -i : identity file;
```

#### Public key default location 
```bash
cat /etc/ssh/sshd_config
    # AuthorizedKeysFile      .ssh/authorized_keys
```

#### Change public key location `cat /etc/ssh/sshd_config`

```bash
AuthorizedKeysFile      /op/authorized_keys

# move public key to another location
mv .ssh/authorized_keys /opt/optauthorized_keys 
```

#### Give ownership of id_rsa file to `user1`, so that `user1` can login with key based authentication.

```bash
cp /root/.ssh/id_rsa /home/user1/    # from root
chown user1 /home/user1/id_rsa       # from root
ssh -i /home/user1/id_rsa root@host1 # from user1
```

> [!WARNING]
> If ssh-keygen generated, then if root passward will be changed, it doesn't affect on key based authentication. 

#### Find public key(authorized_keys) in host
```bash
find / -iname authorized_keys
```
#### Delete public key from host
```bash
find / -iname authorized_keys -exec rm -fr {} \;
```

