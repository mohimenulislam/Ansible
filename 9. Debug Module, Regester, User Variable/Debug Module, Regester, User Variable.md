
### Debug Module 

The `debug` module in Ansible is used to print messages or variable values to help you understand what’s going on in your playbook. It’s especially helpful for troubleshooting or seeing what's inside a variable.

```
ansible servera -m debug

```

```
---
- name: Debuging
  hosts: servera
  tasks:
   - name: Debug something
     debug:
```


```
---
- name: Debuging
  hosts: servera
  tasks:
   - name: Debug something
     debug:
      msg: "Welcome to Ansible Class"
```

```
---
- name: Debuging
  hosts: servera
  tasks:
   - name: Execute shell script to count user in servera
     shell: wc -l /etc/passwd

   - name: Debug something
     debug:
      msg: "Welcome to Ansible Class"
```
```
ansible-playbook debug.yml -v
ansible-playbook debug.yml -vv
ansible-playbook debug.yml -vvv
```

```
---
- name: Debuging
  hosts: servera
  tasks:
   - name: Execute shell script to count user in servera
     shell: wc -l /etc/passwd
     register: myresult

   - name: Debug something
     debug:
      msg: "{{myresult}}"
```

Starting time
```
---
- name: Debuging
  hosts: servera
  tasks:
   - name: Execute shell script to count user in servera
     shell: whoami
     register: myresult

   - name: Debug something
     debug:
      msg: "{{myresult.start}}"
```

```
---
- name: Debuging
  hosts: servera
  tasks:
   - name: Execute shell script to count user in servera
     shell: whoami
     register: myresult

   - name: Debug something
     debug:
      msg:
       - "{{myresult.start}}"
       - "{{myresult.stdout}}"

```

After Local Login: `/etc/motd`
Before Login: `/etc/issue` >> Screen Dsplay



### Variable

Ansible supports variables that can be used to store values that can then be reused throughout files in an Ansible project. This can simplify the creation and maintenance of a project and reduce the number of errors.

Variables provide a convenient way to manage dynamic values for a given environment in your Ansible project. Examples of values that variables might contain include:

- Users to create  
- Packages to install  
- Services to restart  
- Files to remove  
- Archives to retrieve from the internet

---

Ansible allows you to define variables from various sources. Here’s a breakdown of common places where variables can be defined:

1. **Command Line**

2. **Inside `playbook.yaml`**
```yaml
vars:
  name: pavel
  roll: 6024
```

3. **External Variable Files**
```yaml
vars_files:
  - file1
  - ./file2
  - /opt/file3
```

4. **Configuration and Host-Specific Files**
- `/root/ansible/ansible.cfg`
- `/root/ansible/host_vars/servera/vfile1.yaml`

5. **Group-Specific Files**
- `/root/ansible/group_vars/dev/dev.yaml`

6. **Inventory File**
- Define variables directly within the inventory.

### Naming Variables

Variable names must start with a letter, and they can only contain letters, numbers, and underscores.

The following table illustrates the difference between invalid and valid variable names:

| Invalid Variable Names | Valid Variable Names   |
|------------------------|------------------------|
| web server             | web_server             |
| remote.file            | remote_file            |
| 1st file               | file_1                 |
| remoteserver$1         | remote_server1         |


#### User Variables in Ansible

User-defined variables in Ansible can be defined in multiple ways. Below is a list of common sources for defining user variables:

1. **Variable in command line** – when executing playbook  
2. **Playbook** – variables defined directly in the playbook  
3. **External Files** – separate YAML files loaded in the playbook  
4. **group_vars** – group-specific variable files  
5. **host_vars** – host-specific variable files  
6. **Inventory File** – variables defined in the inventory itself



```
---
- name:
  hosts: servera
  tasks:
   - name: Testing value from variable
     debug:
      msg: "{{name}}"
```
``` 
ansible-playbook variable.yml -e name=pathshala
```
```
---
- name:
  hosts: servera
  vars:
   name: From-Play-Book
  tasks:
   - name: Testing value from variable
     debug:
      msg: "{{name}}"
```
```
ansible-playbook variable.yml 
ansible-playbook variable.yml -e name=from-cli # it will execute command
```

#### Create two file `vtest` & vfile in `/home/devops/ansible/`
In `vtest` file
```
---
- name:
  hosts: servera
  vars:
   name: From-Play-Book
  vars_files:
   - /home/devops/ansible/vfile1
  tasks:
   - name: Testing value from variable
     debug:
      msg: "{{name}}"
```

In `vfile1`
```
---
name: FROM-EXTERNEL-FILE
```

```
ansible-playbook vtest
```


### Hosts_Vars

- create a directory name `host_vars` in `/home/devops/ansible/`
- create a directiry name `servera` in `/home/devops/ansible/host_vars`
- create a file name `servera` in `/home/devops/ansible/host_vars/servera`

In `servera` file
```
name: From-Host-Vars-Files
```

/home/devops/ansible/variable.yaml 
```
---
- name:
  hosts: servera
  vars:
  vars_files:
  tasks:
   - name: Testing value from variable
     debug:
      msg: "{{name}}"
```
```
ansible-playbook variable
```


### Group_Vars

create a directory name group_vars in `/home/devops/ansible/`
create a directiry name `dev`, `test` in `/home/devops/ansible/group_vars`
create a file name `dev` in `/home/devops/ansible/group_vars/dev`

In dev file
```
name: From-Group-Vars
```

```
---
- name:
  hosts: servera
  vars:
  vars_files:
  tasks:
   - name: Testing value from variable
     debug:
      msg: "{{name}}"
```
```
ansible-playbook variable
```


#### Inventory 

From Inventory file 
```
[dev]
servera name=From-Inventory
```

```
---
- name:
  hosts: servera
  vars:
  vars_files:
  tasks:
   - name: Testing value from variable
     debug:
      msg: "{{name}}"
```
```
ansible-playbook variable
```


#### CLI >> ExternalFile >> Playbok >> Host_vars >> Group_vars >> Inventory 
#### Host_vars >> Group_Vars


