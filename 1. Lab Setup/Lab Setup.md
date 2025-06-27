
## Lab Setup

### Types of Ansible 
- Ansible Community Edition 
- Red Hat Ansible
- Ansible-core

![image](https://github.com/mohimenulislam/Ansible/blob/1bd8f79c66275f322f41caba636596bc23ce85ce/Img/ansiblemanagement.png)


## Installation 
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

Check Ansible & Python installed or not
```
rpm -qa | grep ansible
rpm -qa | grep python
yum search ansible
rpm -q ansible-core
```

Install Ansible Core
```bash
yum search ansible
yum install ansible-core -y      # minimal version
rpm -q ansible-core            # check

```
Check 
```bash
ansible      # press 'tab' button
ansible --version
```
#### Control node	<br>
- Host name: controlnode<br>
- IP: 192.168.11.100<br>
#### Host1
- Host name: host1
- IP: 192.168.11.101
#### Host2
- Host name: host2
- IP: 192.168.11.102


Set hosts information at controlnode (As we ping with host name)
```bash
vi /etc/hosts
      192.168.11.100  controlnode
      192.168.11.101  host1	
      192.168.11.102  host2
```
Same things do for host1 & host2

Now check form controlnode 
```
ping host1
```

> [!NOTE]
Ansible not run as a servie, ansible just give a command line utility

