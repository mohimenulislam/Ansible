## YAML

Starting with `---` 

```
---
- name: Deploy Server    # play1
  hosts: host1
  tasks:
    - task1 >> name: Installink https package
    - task2
    - task3

- name:    # play2
  hosts:servera
  tasks:
    - task1
    - task2
    - task3
```



```

[devuser1@controlnode myproject]$ vi playbook.yaml
```
Don't use  `Tab`
```
---
- name: Deploy web Server    # play1 
  hosts: host1
  tasks:
   - name: Installink httpd  #task1
     yum: name=httpd state=latest
   - name: Start & Enabled httpd service  #task2
     service: name=httpd state=started enabled=yes
   - name: Open service in firewalld  #task3
     firewalld: service=http state=enabled permanent=yes immediate=yes

```

```

[devuser1@controlnode myproject]$ ansible-playbook --help
[devuser1@controlnode myproject]$ ansible-playbook playbook.yaml --syntax-check
[devuser1@controlnode myproject]$ ansible-playbook playbook.yaml -C   #dry run
```
![image](https://github.com/mohimenulislam/Ansible/blob/db8ed79ec6873ceb816807511c78733535a94730/Img/playbook-c.png)

changed: http not installed in `host1`

Run plabook
```
[devuser1@controlnode myproject]$ ansible-playbook playbook.yaml
```


