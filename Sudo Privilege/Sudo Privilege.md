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

pavel >> /usr/bin/ansitest >> system dekhbe pavel <br>
pavel run as alex <br>



```bash
useradd alex # check from pavel user
udo useradd alex
```

 check sudo user 
```bash
sudo -l #check from user pavel
```


 
F1 = User or Group
F2 = Hosts
F3 = User and Group
F4 = Commands

F1  F2=F(F3)  F4  

pavel ALL=

F1 =  <br>
F2 = ekhane pavel active directory r user o hote pare, active directory r user hole je kono pc theke tar user id, pass de loin korte pare. akhon jodi alex ke sudo privilege dea hoi taile ki se tar nijer machine theke kaj korbe, na bonfhur machine theke kaj korbe na sob machine theke kaj korbe. 
akhon jodi F1 = pavel, F2 = host5 hoi -- tar mane pavel user ki jodi kokhno sudo privilegemdea hoi and sei machine er nam jodi `host5` hoi taholei sudhu sei command execute korbe. by deafault `all` thake. `ALL` = all host. <br>


F3= kon user hisebe switch korbe / ekhane F3= alex hoi ,,, tahole pavel, alex hisebe run korbe je kono system e/  pavel je kono system a run korte parbe as a` alex`hisebe <br>

pavel  ALL = alex lptesting1 <br>
user pavel je kono system a alex hisebe  lptesting1 command  execute korte parbe.

tar mane pavel lptesting1 execute korle system dekhbe seta execute hoyeche alex hisebe
 

pavel  ALL = alex:cisco lptesting 


