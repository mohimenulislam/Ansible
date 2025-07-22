### Template Module
In Ansible, the template module is used to copy Jinja2 template files (usually with a .j2 extension) from your local system (the control node) to the remote managed nodes. It allows you to dynamically generate configuration files or scripts by replacing placeholders with variables.


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
The handlers module in Ansible is used to run specific tasks only when something changes.

If there is new content in `index.html` then `httpd` service will restarted <br>
*** `handlers` is a task & it will execute when `notify` will informed.


