
Group  >>   Primary (Private) <br>
            Secondary (Suplimentary)

A user must have a primary group.
user1 >> primary group 


#### User Module 
```
ansible-doc user
ansible-doc group
```

#### Create User
```
ansible servera -m user -a "name=mokles"
```

#### Remove user
```
ansible servera -m user -a "name=mokles state=absent"
```

#### Remove user with home directory
```
ansible servera -m user -a "name=mokles state=absent remove=yes"
```

#### Assign multiple valus
group->primary
groups->Secondary
```
ansible servera -m user -a "name=mokles state=present uid=1020  shell=/bin/sh comment=NetworkTeam groups=cisco"
```

#### Assign user to another group.
```
ansible servera -m user -a "name=mokes  groups=hpe append=yes" 
```

#### when we creat a user for system uses purpose, then must `system=true`. Therefore system is will be less the 1000



#### Create `cisco` group
```
ansible servera -m group -a "name=cisco state=present"
```

#### Delete `cisco` group
```
ansible servera -m group -a "name=cisco state=absent"
```


### Answer Question

```
[devops@workstation ansible]$ ansible servera -a "grep cisco /etc/group"
```

#### 01. Create user using playbook called user1.yaml

    - Usernmae: user1
    - password: centos321
    - uid: 1050
    - primaryGroup: user1
   
**Ans:** 
```
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

```
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
