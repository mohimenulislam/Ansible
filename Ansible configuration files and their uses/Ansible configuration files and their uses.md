# Ansible configuration files and their uses

## Ansible have multiple configuration file location  

-	Environment Variable (ANSIBLE_CONFIG=)
-	Current working directory (From where you are providing the ansible command)
-	Home Directory of user 
-	/etc/ansible/ansible.cfg


## Current working directory

Suppose we have two projects named `IRIS` and `DBSS`. We can create an `ansible.cfg` file under this directory.

Lets Create two directory from controlnode 
```bash
mkdir IRIS
mkdir DBSS

cd IRIS
touch ansible.cfg
ansible --version   # from IRIS directory
```
