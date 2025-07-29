
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
```
