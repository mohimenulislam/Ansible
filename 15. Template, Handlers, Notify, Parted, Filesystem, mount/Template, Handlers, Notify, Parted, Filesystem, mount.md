### Template Module

```bash
ansible-doc template
```

index.html
```yaml
welcome to "{{ansible_fqdn}}"

#or
FQDN =  {{ ansible_fqdn }}
IP= {{ ansible_all_ipv4_addresses }}
ARCH= {{ ansible_architecture }}
```

```yaml
---
- name: Build Webserver
  hosts: dev, test
  tasks:
   - name: Install httpd
     yum:
      name: httpd
      state: latest

   - name: start and enable httpd service
     service:
      name: httpd
      state: started
      enabled: true

   - name: Allow http for all
     firewalld:
      service: http
      state: enabled
      immediate: yes
      permanent: yes

   - name: Copying servera.html to servera
     template:
      src: ./index.html
      dest: /var/www/html/index.html
```


### Background study
#### Installinh httpd package
```yaml
---
- name: Build Webserver
  hosts: dev, test
  tasks:
   - name: Install httpd
     yum:
      name: httpd
      state: latest

   - name: start and enable httpd service
     service:
      name: httpd
      state: started
      enabled: true

   - name: Allow http for all
     firewalld:
      service: http
      state: enabled
      immediate: yes
      permanent: yes

   - name: Copying servera.html to servera
     copy:
      src: ./servera.html
      dest: /var/www/html/index.html
     when: ansible_hostname=="servera"

   - name:  Copying serverb.html to serverb
       copy:
        src: ./serverb.html
        dest: /var/www/html/index.html
       when: ansible_hostname=="serverb"
```


### handlers
If there is new content in index.html then httpd service will restarted <br>
*** `handlers` is a task & it will execute when `notify` will informed.


