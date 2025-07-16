
Group  >>   Primary (Private) <br>
            Secondary (Suplimentary)

A user must have a primary group.
user1 >> primary group 


#### User Module 
```bash
ansible-doc user
ansible-doc group
```

#### Create User
```bash
ansible servera -m user -a "name=mokles"
```

#### Remove user
```bash
ansible servera -m user -a "name=mokles state=absent"
```

#### Remove user with home directory
```bash
ansible servera -m user -a "name=mokles state=absent remove=yes"
```

#### Assign multiple valus
group->primary
groups->Secondary
```bash
ansible servera -m user -a "name=mokles state=present uid=1020  shell=/bin/sh comment=NetworkTeam groups=cisco"
```

#### Assign user to another group.
```bash
ansible servera -m user -a "name=mokes  groups=hpe append=yes" 
```

#### when we creat a user for system uses purpose, then must `system=true`. Therefore system is will be less the 1000



#### Create `cisco` group
```bash
ansible servera -m group -a "name=cisco state=present"
```

#### Delete `cisco` group
```bash
ansible servera -m group -a "name=cisco state=absent"
```


### Answer Question

```bash
[devops@workstation ansible]$ ansible servera -a "grep cisco /etc/group"
```

#### 01. Create user using playbook called user1.yaml

    - Usernmae: user1
    - password: centos321
    - uid: 1050
    - primaryGroup: user1
   
**Ans:** 
```yaml
---
- name: Create user1, pass, uid, primary group
  hosts: servera
  vars:
   mypass: centos321
  tasks:
   - name: creating user1
     user:
      name: user1
      uid: 1050
      password: "{{ mypass | password_hash('sha512') }}"

```

#### 02. Create user using playbook called user2.yaml

    - Usernmae: user2
    - password: centos321
    - uid: 1051
    - shell: /bin/sh
    - homeDirectory: /opt/user2
    - primaryGroup: user2
    - SecondaryGroup: cisco

**Ans:**

```yaml
---
- name: Create user1, pass, uid, primary group
  hosts: servera
  vars:
   mypass: centos321
  tasks:
   - name: Creating Group
     group:
      name: cisco
      state: present

   - name: creating user1
     user:
      name: user1
      uid: 1050
      shell: /bin/bash
      home: /opt/user2
      password: "{{ mypass | password_hash('sha512') }}"
      group: cisco
      state: present
      append: yes
```

#### 03. Create a file userlist.yaml with below contents.

    userlist:
      - user10
      - user11
      - user12
      - user13
      - user14
      - user15

**Ans**
Create file name `userlist.yaml`
```
userlist:
 - user10
 - user11
 - user12
 - user13
 - user14
 - user15
```

#### 04. Create users using the the variables "userlist" from userlist.yaml file.. Playbook name should be multiuser1.yaml and password of all users should be TomBigBee.

Create file name `multiuser1.yaml`
```yaml
---
- name: Multiuser
  hosts: servera
  vars:
   - mypass: TomBigBee
  vars_files:
   - ./userlist.yaml
  tasks:
   - name: Creteing multiuser
     user:
      name: "{{ item }}"
      password: "{{ mypass | password_hash('sha512') }}"
      state: present
     with_items: "{{ userlist }}"

```

#### 05. create a file userdetails.yaml with the below

userinfo:
  - name: devuser1
    pass: dev321
    shell: /bin/bash

  - name: devuser2
    pass: dev123
    shell: /bin/bash
    
  - name: devuser3
    pass: pass321
    shell: /sbin/nologin

**Ans:**
Create a filne name `userdetails.yaml`
```
userinfo:
  - name: devuser1
    pass: dev321
    shell: /bin/bash

  - name: devuser2
    pass: dev321
    shell: /bin/bash

  - name: devuser3
    pass: pass321
    shell: /sbin/nologin
```

#### 06. Create users using the the variables "userinfo" from userdetails.yaml file. Use all the items of variable while creating user. Playbook name should be multiuser2.yaml.

Create a file name `multiuser2.yaml`
```yaml
---
- name: Creating multiuser
  hosts: servera
  vars_files:
   - ./userdetails.yaml
  tasks:
   - name: Creating module
     user:
       name: "{{ item.name }}"
       shell: "{{ item.shell }}"
       password: "{{ item.pass | password_hash('sha256') }}"
     loop: "{{ userinfo }}"
```
