# Ansible configuration files and their uses

## Ansible has multiple configuration file locations, ordered by priority.

-	Environment Variable (ANSIBLE_CONFIG=)
-	Current working directory (From where you are providing the ansible command)
-	Home Directory of user (~/.ansible.cfg)
-	/etc/ansible/ansible.cfg

## Environment Variable (ANSIBLE_CONFIG=)

We can define `ansible.cfg` in environment variable.
From Controlnode
```bash
touch /opt/ansible.cfg
ANSIBLE_CONFIG=/opt/ansible.cfg
export ANSIBLE_CONFIG
env | grep -i ansible.cfg
ansible --version 
```


## Current working directory

Suppose we have two projects named `IRIS` and `DBSS`. We can create an `ansible.cfg` file under this directory.

#### Lets Create two directory from controlnode 
```bash
mkdir IRIS
mkdir DBSS

cd IRIS
touch ansible.cfg
ansible --version   # from IRIS directory
```

![image](https://github.com/mohimenulislam/Ansible/blob/d52758485aca0e80c7658a2e9eda573846a4b8e2/Img/Current%20working%20directory.png)

#### We can also create ansible.cfg in any other user

```bash
useradd devuser1
su - devuser1
```

Let say we have project named `myproject` and create `ansible.cfg` file under `myproject` directory.

```bash
mkdir myproject
cd myproject
touch ansible.cfg
ansible --version
```

Define host file 

```bash
touch myhost    # under myproject directory
vi ansible.cfg

  [defaults]
  inventory=myhost
  # or
  inventory=./myhost  # recommended 
  # or
  inventory=/home/devops/myproject/myhost
```

We can also create multiple host file 
```bash
touch myhost2   # under myproject directory
ansible all --list-hosts -i myhost2
```

Run the conmmand
```bash
ansible all -m ping 
```
![image](https://github.com/mohimenulislam/Ansible/blob/4e4396e910b9d3ca7d36c9148f131acc22c26421/Img/devuser1.png)

Here, `devuser1` tries to log in to host1 and host2 as the `devops` user.

```bash
vi myproject/ansible.cfg

  [defaults]
  inventory=./inventory
  remote_user=devops

ansible all -m ping
```
![image](https://github.com/mohimenulislam/Ansible/blob/c629d2ac497517e62d2984f2f8ef8dc50b165220/Img/devops_user.png)

Generate key-base authentication (From Controlnode, devuser1). The `devops` user from host1 and host2 already has sudo privileges.

```bash
ssh-keygen
ssh-copy-id devops@host1
ssh-copy-id devops@host2
ansible all -m ping
```

![image](https://github.com/mohimenulislam/Ansible/blob/e2c738b3a09ed185048bb32cd95454313dd6aa0b/Img/ping.png)

```bash
ansible all -a hwclock
ansible all -a hwclock -vvv
```
![image](https://github.com/mohimenulislam/Ansible/blob/03fbe334fa9291b9d4edb0c044a095e30335fa44/Img/hwclock.png)

Here, `devops` user become as `root`
```bash
vi myproject/ansible.cfg

  [defaults]
  inventory=./inventory
  remote_user=devops

  [privilege_escalation]
  become=true
  become_user=root
  become_method=sudo
  become_ask_pass=false
```

## Change `SSH` port 

### Step 
- Change port to /etc/ssh/sshd_config file
- Add port to selinux (if it's there)
- Reload sshd
- change the port to firewall
- Reload firewalld


```bash
vi /etc/ssh/sshd_config    # from host2

  Port 29

sshd -T
systemctl restart sshd
journalctl -xe

### If we want to listen to a service on a different port...
getenforce
semanage port -l | grep ssh
man semanage port
semanage port -a -t ssh_port_t -p tcp 29
semanage port -l | grep ssh
systemctl restart sshd
netstat -tunpal

### Change the port to firewall
firewall-cmd --status
firewall-cmd --list-all
firewall-cmd --permanent --remove-service=ssh
firewall-cmd --permanent --add-port=29/tcp
firewall-cmd --reload 
firewall-cmd --list-all

ssh -p 29 localhost
```

Try to ssh 
```bash
ssh root@192.168.0.102 -p 29
# or
ssh devops@192.168.0.102 -p 29
```

```bash
ansible all -m ping
```
![image](https://github.com/mohimenulislam/Ansible/blob/584401c681ed6b1abde902ab44a9830cabbfeb84/Img/sshportchange.png)


```bash
vi myproject/myhost

  [webserver]
  host1
  host2 ansible_port=29
```

```bash
ansible all -m ping
```
![image](https://github.com/mohimenulislam/Ansible/blob/909d8420de2204aae1cb337204ea0e1184135b04/Img/port29.png)

### If `host2` says that the devops user is not allowed...



### After changing the SSH port from 22 to 29 in `host2, if you try ssh-copy-id from the control node...
```bash
ssh-copy-id -o port=29 pavel@host2
```

## Background Study 
We know all command in linux are  stored in `/usr/bin` or `/usr/sbin` like `/usr/sbin/useradd`.
### How Does the Shell Find useradd?
```bash
which useradd
```

### How Does the Shell Know Where to Find useradd?

When you type a command (e.g., `useradd`), the shell doesn’t magically know where it is—it searches for it using the `$PATH` environment variable.

### What is $PATH?

- `$PATH` is an environment variable that contains a list of directories separated by colons (`:`).
- The shell searches these directories in order to find the command you entered.
- You can check your $PATH with:

```bash
echo $PATH
```
 
### Example output:

```bash
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```
This means the shell will look for commands in these directories, in this order.
