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

```
---
- name: Deptop httpd server
  hosts: host1
  tasks:
   - name: Installing httpd
     yum:
      name: httpd
      state: latest

   - name: Start and enable httpd service
     service:
      name: httpd
      state: started
      enabled: yes

   - name: Add hhtpd service in firewalld
     firewalld:
      service: http
      state: enabled
      permanent: yes
      immediate: yes

   - name: Copy Index File
     copy:
      src: /var/www/html/index.html
      dest: /var/www/html/

```

Absent & copy file delete
```
--
- name: Installing httpd web service
  hosts: host1
  tasks:
   - name: Installing httpd
     yum:
      name: httpd
      state: absent

   - name: delete file
     shell: |
      rm -rf /var/www/html/index.html
```


