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

```bash
vi myproject/ansible.cfg

  [defaults]
  inventory=./inventory
  remote_user=devops

ansible all -m ping
```
![image](https://github.com/mohimenulislam/Ansible/blob/20ed9ff52f1d580fd4ec1343ae1e1136d0a16929/Img/devopsuser.png)


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
