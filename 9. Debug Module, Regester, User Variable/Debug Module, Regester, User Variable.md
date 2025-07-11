
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



After Local Login: `/etc/motd`
Before Login: `/etc/issue` >> Screen Dsplay
