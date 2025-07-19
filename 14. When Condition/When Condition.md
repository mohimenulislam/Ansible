

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
     when: ansible_hostname=="serverb"
```
```bash
ansible-playbook usercreate1.yaml -C
```


Or (Only applicable for `AND` operation)
```
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
- user 'testadm1' should be created in test hostgroup.

