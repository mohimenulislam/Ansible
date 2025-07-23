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

Create `servera.html` & `serverb.html` file and write something
```bash
vi servera.html
vi serverb.html
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


### handlers & notify
The handlers module in Ansible is used to run specific tasks only when something changes.

If there is new content in `index.html` then `httpd` service will restarted <br>
*** `handlers` is a task & it will execute when `notify` will informed.

Create file `index.html` & write something
```bash
vi index.html
```

```yaml
---
- name: Build Webserver
  hosts: servera
  tasks:
   - name: Install httpd
     yum:
      name: httpd
      state: present
   - name: Start and enable httpd service
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
   - name: Copying index.html to servera
     template:
      src: ./index.html
      dest: /var/www/html/index.html
     notify:
      - restart httpd

  handlers:
   - name: restart httpd
     service:
       name: httpd
       state: restarted
```



### Mail 

If not installed
```bash
ansible-galaxy collection install community.general
ansible-doc mail
```

```
yum install mailx
#or
tail -f /var/spool/mail/root
#or
mail
```

Create file `mail.yaml`
```yaml
---
- name: Build Webserver
  hosts: servera
  tasks:
   - name: Install httpd
     yum:
      name: httpd
      state: present
   - name: Start and enable httpd service
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
   - name: Copying index.html to servera
     template:
      src: ./index.html
      dest: /var/www/html/index.html
     notify:
      - restart httpd

  handlers:
   - name: restart httpd
     mail:
      subject: index.html has been changed successfully......
      delegate_to: localhost
      name: httpd
      state: restarted
```

### Parted, filesystem, mount
- fstab
- present >> add entry to /etc/fstab  
- absent >> remove entry from /etc/fstab  
- mounted >> add entry to /etc/fstab < if entry is not already there > and mount the volume 

**Creating partition** 

```yaml
---
- name: Creating Primary pertition
  hosts: servera, serverb
  tasks:
    - name: Creating /dev/sda1
      parted:
       device: /dev/sda
       number: 1
       part_end: 200MiB
       state: present
    - name: Create FileSystem
      filesystem:
       dev: /dev/sda1
       fstype: xfs
    - name: Mounting Filesystem
      mount:
       src: /dev/sda1
       path: /mnt/data
       fstype: xfs
       state: mounted
```

**Delete partition**
```yaml
---
- name: Creating Primary pertition
  hosts: servera, serverb
  tasks:
    - name: Creating /dev/sda1
      parted:
       device: /dev/sda
       number: 1
       part_end: 200MiB
       state: absent

  pre_tasks:
   - name: Mounting Filesystem
     mount:
      src: /dev/sda1
      path: /mnt/data
      fstype: xfs
      state: unmounted
```
