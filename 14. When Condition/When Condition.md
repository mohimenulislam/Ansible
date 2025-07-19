

#### 1. Create a playbook named `usercreate1.yaml` and create the user with below details
- user support1 should be created in all hosts.
- user 'usera' in servera host and if os-distribution is ubuntu
- user 'userb' in serverb host.

**Ans**

```bash
ansible servera -m setup
ansible servera -m setup | grep -i dis
```

```yaml
---
- name: Creating user
  hosts: all
  tasks:
   - name: Creating user support1 in all hosts
     user:
      name: support1
      state: present

   - name: Creating user usera in severa host
     user:
      name: usera
      state: present
     when: ansible_hostname=="servera" and ansible_distribution=="RedHat"

   - name: Creating user userb in severa host
     user:
      name: userb
      state: present
     when: ansible_hostname=="serverb"  # or ("'serverb' in ansible_host")
```
```bash
ansible-playbook usercreate1.yaml -C
```


Or (Only applicable for `AND` operation)
```yaml
---
- name: Creating user
  hosts: all
  tasks:
   - name: Creating user support1 in all hosts
     user:
      name: support1
      state: present

   - name: Creating user usera in severa host
     user:
      name: usera
      state: present
     when:
       - ansible_hostname=="servera"
       - ansible_distribution=="RedHat"

   - name: Creating user userb in severa host
     user:
      name: userb
      state: present
     when: ansible_hostname=="serverb"
```
```bash
ansible-playbook usercreate1.yaml -C
```


#### 2. Create a playbook named usercreate2.yaml and create users with below details
- user noc1 should be created in all hosts
- user 'devadm1' should be created in dev hostgroup
- user 'testadm1' should be created in test hostgroup. (if not in this group)

**Background study**
**fixed variable check**
```bash
ansible servera -m debug -a "var=ansible_host"    # fixed variable
ansible servera -m debug -a "var=ansible_jonjalipara"  # not define
```

```yaml
---
- name: servera
  hosts: servera
  tasks:
   - name: Debug
     debug:
      var: vars
```

```bash
ansible-playbook debugmsg.yaml -C # pint all variable
```
```bash
ansible-playbook debugmsg.yaml | grep group_names
```

**Ans:**

```yaml
---
- name: Creating users
  hosts: all
  tasks:
   - name:
     user:
      name: noc1
      state: present
   - name: Creating user devadm1 in dev host group
     user:
      name: devadm1
      state: present
     when: "'dev' in group_names"
   - name:  Creating user textadm1 in text hostgroup
     user:
      name: testadm1
      state: present
     when: "'test' in group_names" # "'test' not in group_names"
```
```
ansible-playbook usercreate2.yaml -C
```

#### 3. Create a playbook web.yaml which will deploy webserver in webservers host group.
- Ensure that external network can access your webserver.
- http://serverc.lab.example.com should provide the web content from http://content.example.com/ansilab/serverc.html as landing page.
- http://serverd.lab.example.com should provide the web content from http://content.example.com/ansilab/serverd.html as landing page.

Note: Ensure that you can access below URLs from your control node.
- curl http://serverc.lab.example.com  
- curl http://serverd.lab.example.com

  **Ans 1:**
```bash
wget http://content.example.com/ansilab/serverc.html
wget http://content.example.com/ansilab/serverd.html
```

  ```yaml
  ---
- name: Installing webserver
  hosts: dev, test
  tasks:
   - name: Installing httpd
     yum:
       name: httpd
       state: latest

   - name: Start & enable httpd service
     service:
       name: httpd
       state: started
       enabled: yes

   - name: open httpd for all
     firewalld:
      service: http
      state: enabled
      permanent: yes
      immediate: yes

   - name: Copy serverc to /var/www/html/index.html in serverc
     copy:
      src: ./serverc.html
      dest: /var/www/html/index.html
     when: "'serverc' in ansible_host" # or ansible_hostname=="serverc"

   - name: Copy serverd to /var/www/html/index.html in serverc
     copy:
      src: ./serverd.html
      dest: /var/www/html/index.html
     when: "'serverd' in ansible_host"  # or ansible_hostname=="serverd"
  ```

**Ans 2:**

```yaml
---
- name: Installing webserver
  hosts: webserver
  tasks:
   - name: Installing httpd
     yum:
       name: httpd
       state: latest

   - name: Start & enable httpd service
     service:
       name: httpd
       state: started
       enabled: yes

   - name: open httpd for all
     firewalld:
      service: http
      state: enabled
      permanent: yes
      immediate: yes

   - name: Copy serverc to /var/www/html/index.html in serverc
     get_url:
      url: http://content.example.com/ansilab/serverc.html
      dest: /var/www/html/index.html
     when: "'serverc' in ansible_host" # or ansible_hostname=="serverc"

   - name: Copy serverd to /var/www/html/index.html in serverc
     get_url:
      url: http://content.example.com/ansilab/serverd.html
      dest: /var/www/html/index.html
     when: "'serverd' in ansible_host"  # or ansible_hostname=="serverd"
```

```bash
curl serverc
curl serverd
```

**Ans 3:**

```yaml
---
- name: Installing webserver
  hosts: webserver
  vars:
   urlc: http://content.example.com/ansilab/serverc.html
   urld: http://content.example.com/ansilab/serverd.html

  tasks:
   - name: Installing httpd
     yum:
       name: httpd
       state: latest

   - name: Start & enable httpd service
     service:
       name: httpd
       state: started
       enabled: yes

   - name: open httpd for all
     firewalld:
      service: http
      state: enabled
      permanent: yes
      immediate: yes

   - name: Copy serverc to /var/www/html/index.html in serverc
     get_url:
       url: "{{ urlc }}"
      dest: /var/www/html/index.html
     when: "'serverc' in ansible_host" # or ansible_hostname=="serverc"

   - name: Copy serverd to /var/www/html/index.html in serverc
     get_url:
      url: "{{ urld }}"
      dest: /var/www/html/index.html
     when: "'serverd' in ansible_host"  # or ansible_hostname=="serverd"
```

```bash
curl serverc
curl serverd
```
