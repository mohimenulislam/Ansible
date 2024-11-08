## Sudo Privilege

> [!NOTE]
> - `sudo`: super user do
> - When we try to local user, ansible by default try to sudo privilege. <br>
> - Sudo like as "Run as administrator"


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

pavel >> useradd >> system will see - pavel <br>
pavel >> useradd >> system will see - root <br>
pavel run as root <br>
 
/usr/bin/ansitest <br>
owner: alex <br>
execution>>alex <br>

pavel >> /usr/bin/ansitest >> system dekhbe - pavel <br>
pavel run as alex <br>



```bash
useradd alex # check from pavel user
udo useradd alex
```



pavel  ALL = alex lptesting1 <br>
user pavel je kono system a alex hisebe  lptesting1 command  execute korte parbe.

tar mane pavel lptesting1 execute korle system dekhbe seta execute hoyeche alex hisebe
 



pavel ALL = alex:ALL lptesting 




```bash
visudo
     # pavel  ALL=(ALL)  ALL
     # pavel ALL=(ALL) NOPASSWD:ALL  # password chaibe na
     # pavel host5=(ALL) NOPASSWD:ALL
     # pavel host1,host5=(ALL) NOPASSWD:ALL
     # pabel host1=(root) NOPASSWD: /usr/bin/ansitest 
```


```bash
sudo -l # check prom pavel user
sudo useradd alelx  # without sudo it doesn't work, must be sudo dite hobe 
```


```bash
vi /usr/bin/ansitest
       # echo "Welcome to ansible 11"
ls -l /usr/bin/ansitest # check ownership
chmod u+x /usr/bin/ansitest
```




```bash
chown alex:alex /usr/bin/ansitest
ls -l /usr/bin/ansitest

```
sudo te kono user er nam define n akorle by default se root hisebe execite hobe 





/usr/bin/ansitest <br>
owner: alex <br>
execution>>alex <br>


How to give permission user pavel to ansitest as a alex <br>
pavel >> useradd >> system dekhbe - pavel <br>
pavel >> useradd >> system dekhbe - root <br>
pavel >> /usr/bin/ansitest >> system dekhbe - pavel <br>

```bash
visudo
      #pavel host1=(alex) NOPASSWD: /usr/bin/ansitest
sudo -u alex ansitest # -u na dile by default root hisebe kal kore 
```


----------------------



cisco >> ccna & ccnp >> sudo pr

```bash
groupadd cisco # from controlnode
useradd ccna
useradd ccnp
visudo
   #%cisco  ALL=(ALL)       NOPASSWD: ALL
```

```bash
sudo -l
sudo ansitest # after joining group logout and login. from user ccna

```

---------------------------------------------------------------------------------------

audit loga sob dekha jai ke kokhon login korse
!v 
