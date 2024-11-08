
## Sudo Privilege

> [!NOTE]
> - `sudo`: super user do
> - When we try to local user, ansible by default try to sudo privilege. <br>
> - Sudo like as "Run as administrator"
> - After sudo privileged, must use `sudo` before command.
> - If any user is not define in sudo, by default it executed as root. Ex: pavel ALL=`(ALL)` ALL



 ### Configuration File 
```bash
/etc/sudoers  # If we give any wronfg input, it will take. 
#or
visudo # recommended; If we give any wrong input, it will remind us.
#or
vi /etc/sudoers.d/<anyname>  # The Standard is, whom user we will give sudo privilege, we will create a file according to the user name.
```


### How we give a user sudo privilege
- F1 = User or Group <br>
- F2 = Hosts <br>
- F3 = User and Group <br>
- F4 = Commands <br>

F1  F2=(F3)  F4 <br>
- `pavel ALL=(ALL) ALL`  # by default <br>
- `pavel host1=(alex) NOPASSWD: /usr/bin/ansitest` <br>
- `%cisco ALL=(ALL) NOPASSWD: ALL`  # for group 


#### Explaination : 
F1 = Which `user`/`group` we will give sudo privilege <br>

F2 = Here, user `pavel` can be active directory user, this user can login from any other computers. <br>
     For example: `pavel host1=(ALL) ALL`, it means - `pavel` user can execute all command only from `host1` node. <br>
     `pavel ALL=(ALL) ALL` it means - `pavel` user can execute all command from all node.<br>

F3 = `pavel ALL=(alex) ALL` - here user `pavel` will run as `alex`  on any system. <br>

F4 = Which command will execute. `pavel ALL=(ALL) /usr/bin/ansitest` - here pavel can execute only `ansitest` command


### Some example of sudo permission are given below
- `pavel ALL=(ALL) ALL`: - from any `host/system` user `pavel` can execute all `commands` as any `user`.
- `pavel ALL=(ALL) NOPASSWD: ALL`: - No passward required.
- `pavel host1=(ALL) ALL`: - from `host1` user `pavel` can execute all `commands` as any `user`.
- `pavel host1,host5=(ALL) ALL`: - from `host1` and `host5` user `pavel` can execute all `commands` as any `user`.
- `pavel ALL=(alex) NOPASSWD: /usr/bin/ansitest `: - from any `host` user `pavel` can execute only `ansitest` command as user `alex`.
- `pavel ALL=(alex) NOPASSWD: /usr/bin/ansitest, /usr/sbin/useradd`: - can execute `ansitest` and `useradd` command.
- `%cisco ALL=(ALL) NOPASSWD: ALL`: - from any `host`, all user of `cisco` group can execute all `commands` as any `user`.
- `pavel  ALL = alex:cisco ALL`: - from any system user `pavel` can execute all commands as a user `alex`, as a group `cisco`


#### Check user is sudo privileged or not 
```bash
sudo -l
```

### Background Study 

------------------------------------------------
### Example 1:

Lets try to create a user `alex` from `pavel` user.
```
useradd alex
```
We can't create `alex` user from pavel because pavel user is not sudo privileged. <br>
Now give sudo privileged to `pavel` user <br> 
```bash
visudo  # from root user
  # pavel ALL=(ALL) ALL
```

From pavel user 
```
sudo -l
useradd alex
```
Now pavel user able to create a user.

---------------------------------------------------
### Example 2:
Create user `alex`

/usr/bin/ansitest <br>
owner: alex <br>
execution>>alex <br>

How to give permission user pavel to ansitest as a alex <br>

from root user
```bash
vi /usr/bin/ansitest
   # echo "welcome to ansible 11"
chmod u+x /usr/bin/ansitest
chown alex:alex /usr/bin/ansitest
ls -l /usr/bin/ansitest

visudo
      # pavel ALL=(alex) NOPASSWD: /usr/bin/ansitest
```

check from pavel user
```bash
sudo -u alex ansitest  # here, pavel run as user `alex`.
```

----------------------

### Example 3:

Create a group `cisco` from `controlnode` <br> 
Create user `ccna` & `ccnp` <br>
Give `cisco` group as sudo privilege <br>

```bash
groupadd cisco 
useradd ccna
useradd ccnp
visudo
   # %cisco ALL=(ALL) NOPASSWD: ALL
```

check from user `ccna`
```bash
sudo -l
sudo ansitest # after joining group logout and login
```



