
## Lab Setup
VMware Workstation 
![image alt](https://github.com/mohimenulislam/Ansible/blob/cf6e821bcb4a33d73a6da8ed9176d9f903781138/Img/vmwareimage.png)
Or Setting >> CD/DVD >> Browse ISO>> Connect 

Check:
```bash
lsblk
```
![image alt](https://github.com/mohimenulislam/Ansible/blob/8136630a82f6773cc2c60ff35b195a0acdd2be74/Img/lsblkaftercdattached.png)

Mount: 
```bash
mount /dev/sr0 /software/
```
![image](https://github.com/mohimenulislam/Ansible/blob/2bbe0ae61282fe742c1f525bf185dc5e9a760283/Img/lsblkaftermount.png)

fstab entry
```
vi /etc/fstab
      dev/sr0                /software               iso9660 defaults        0 0
mount -av
```

Yum Configure 
```bash
vi /etc/yum.repos.d/redhat.repo
      [AppStream]
      name=AppStream
      baseurl=file:///software/AppStream
      gpgcheck=0

      [BaseOS]
      name=BaseOS
      baseurl=file:///software/BaseOS
      gpgcheck=0

yum repolist all
yum list available
```

Install bash completion
```bash
yum install bash-completion
```

### Control node	<br>
- Host name: controlnode<br>
- IP: 192.168.11.100<br>
### Host1
- Host name: host1
- IP: 192.168.11.101
### Host2
- Host name: host2
- IP: 192.168.11.102

Set hosts information at controlnode
```bash
vi /etc/hosts
      192.168.11.100  controlnode
      192.168.11.101  host1	
      192.168.11.102  host2
```

Same things do for host1 & host2

