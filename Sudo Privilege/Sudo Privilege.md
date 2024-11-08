## Sudo Privilege

> [!NOTE]
> - `sudo`: super user do
> - When we try to local user, ansible by default try to sudo privilege. <br>
> - Sudo like as "Run as administrator"


 ### Configuration File 
```bash
/etc/sudoers  # kono vul input dile o nibe 
#or
visudo # recommended 
#or
vi /etc/sudoers.d/<anyname>
```

pavel >> useradd >> system dekhbe - pavel <br>
pavel >> useradd >> system dekhbe - root <br>
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

 check sudo user 
```bash
sudo -l #check from user pavel
```


 
F1 = User or Group <br>
F2 = Hosts <br>
F3 = User and Group <br>
F4 = Commands <br>

F1  F2=(F3)  F4  

pavel ALL=

F1 =  <br>
F2 = ekhane pavel active directory r user o hote pare, active directory r user hole je kono pc theke tar user id, pass de loin korte pare. akhon jodi alex ke sudo privilege dea hoi taile ki se tar nijer machine theke kaj korbe, na bonfhur machine theke kaj korbe na sob machine theke kaj korbe. 
akhon jodi F1 = pavel, F2 = host5 hoi -- tar mane pavel user ki jodi kokhno sudo privilegemdea hoi and sei machine er nam jodi `host5` hoi taholei sudhu sei command execute korbe. by deafault `ALL` thake. `ALL` = all host. <br>


F3= kon user hisebe switch korbe / ekhane F3= alex hoi ,,, tahole pavel, alex hisebe run korbe je kono system e/  pavel je kono system a run korte parbe as a` alex`hisebe <br>

pavel  ALL = alex lptesting1 <br>
user pavel je kono system a alex hisebe  lptesting1 command  execute korte parbe.

tar mane pavel lptesting1 execute korle system dekhbe seta execute hoyeche alex hisebe
 

pavel  ALL = alex:cisco lptesting1
pavel user je kono system a lptesting1 command execute korte parbe as a user alex hisebe as a group cisco hisebe

pavel ALL = alex:ALL lptesting 




```bash
visudo
     # pavel  ALL=(ALL)  ALL
     # pavel ALL=(ALL) NOPASSWD:ALL  # password chaibe na
     # pavel host5=(ALL) NOPASSWD:ALL
     # pavel host1,host5=(ALL) NOPASSWD:ALL
     # pabel host1=(root) NOPASSWD: /usr/bin/ansitest 
```
audit loga sob dekha jai ke kokhon login korse

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

!v 


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
      #pabel host1=(alex) NOPASSWD: /usr/bin/ansitest
sudo -u alex ansitest # -u na dile by default root hisebe kal kore 
```


----------------------
/etc/sudoers.d/


standard hocche je user ke privilege dite chacchi sei user er nam 

vi /etc/sudoers.d/pavel


  ---------------------------

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

  
