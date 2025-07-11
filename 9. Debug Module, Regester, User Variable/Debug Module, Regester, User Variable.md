
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
