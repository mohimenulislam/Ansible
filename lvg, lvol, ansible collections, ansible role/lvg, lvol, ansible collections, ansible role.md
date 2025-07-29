
### Partition two types:- 

- Standard Partition (also called Primary/Extended)
  - Traditional disk partition type.
  - Fixed in size — once created, you cannot reduce or expand it easily (especially when mounted).
  - Managed via tools like fdisk, parted, or gparted.
  - Maximum of 4 primary partitions, or 3 primary + 1 extended (which can contain multiple logical partitions)

- LVM (Logical Volume Manager)
  - Logical abstraction over physical disks.
  - Partitions are created as Volume Groups and Logical Volumes.
  - You can:
   - Resize (increase/decrease) volumes dynamically.
   - Add or remove physical storage on the fly.
   - Take snapshots for backups.
 
```bash
ansible-doc lvg
ansible-doc -l | grep lvg
```

```bash
[devops@workstation ansible]$ ansible servera -m lvg -a "vg=vg0 pvs=/dev/sda"
```
**From servera check**
```bash
pvs
vgs
```

```bash
[devops@workstation ansible]$ ansible servera -m lvol -a "vg=vg0 lv=lv1 size=200m state=present"
```

**From servera check**

```bash
lsblk /dev/vg0/lv1
blkid /dev/vg0/lv1 # No filesystem
```


**Filesystem Create**
```
[devops@workstation ansible]$ ansible servera -m filesystem -a "device=/dev/vg0/lv1 fstype=xfs"
```

**From servera check**
```bash
blkid /dev/vg0/lv1
```

**Delete lvm**
```bash
[devops@workstation ansible]$ ansible servera -m lvol -a "vg=vg0 lv=lv1 state=absent force=yes"
```

**Delete vg & PV**
```bash
[devops@workstation ansible]$ ansible servera -m lvg -a "vg=vg0 state=absent"
[devops@workstation ansible]$ ansible servera -a "pvremove /dev/sda"
```

**From servera check**
```bash
pvs
vgs
lvs
```

```yaml
---
- name: Creating LVM
  hosts: servera
  tasks:
   - lvg:
      vg: myvg1
      pvs: /dev/sda
      state: present
      pesize: 8m

   - lvol:
      lv: lv1
      vg: myvg1
      size: 100m

   - filesystem:
      device: /dev/myvg1/lv1
      fstype: xfs

   - mount:
      src: /dev/myvg1/lv1
      path: data1
      fstype: xfs
      state: mounted
```


```bash
ansible-playbook lv.yaml -C # may error
ansible-playbook lv.yam
```


**lvresize**
???



### Background Study 
Command two types:
- ad-hok
- playbook (yaml)

### Ansible Role & Collection

**Role**
Ansible Role = Pre-defined folder structure + modular tasks
- Reusability: Write once, reuse in many playbooks.
- Organization: Break large playbooks into logical parts.
- Scalability: Clean code separation for teams or big infrastructures.

Role >> webserver
Role >> dbserver

 
**Ansible Collections**

- module << copy, lvg >>
- plugins << ansible_connections >>
- role << -mysql, - dbrole >>


A complete package of reusable Ansible automation components <br>
Think of it like a Python package for Ansible — instead of installing just one role or module, you get an entire toolkit.
