# Ansible Module 

```bash
ansible-doc -l

ansible-doc-l | wc -l

ansible-doc-l | grep command

ansible-doc -l | grep -i cisco
```

Commonly used Ansible Modules 
- ping
- win_ping
- command
- shell
- copy
- fetch
- yum
- service
- firewalld


## Command Module 
The `command` module takes the command name followed by a list of space-delimited arguments. The given command will be executed on all selected nodes. The command(s) will not be processed through 
the shell, so variables like `$HOSTNAME` and operations like `"*"`, `"<"`, `">"`, `"|"`, `";"` and `"&"` will not work. Use the [ansible.builtin.shell] module if you need these features. To create `command` 
tasks that are easier to read than the ones using space-delimited arguments, pass parameters using the `args` task keyword <https://docs.ansible.com/ansible/latest/reference_appendi ces/playbooks_keywords.html#task> or use `cmd` parameter. 
Either a free form command or `cmd' parameter is required, see the examples. For Windows targets, use the [ansible.windows.win_command] module instead.

## Command Module 
 
```bash
ansible-doc-l | grep command
ansible-doc command

ansible all -m command -a "date"
ansible all -m command -a "date" -a "cal"
ansible all -m command -a "yum repolist"
ansible all -m command -a "yum install httpd -y"


watch ls -ld /backup  #host1
ansible host1 -m command -a "mkdir /backup"
ansible host1 -m command -a "touch /backup/file1"
ansible host1 -m command -a "echo welcome > /backup/file1"    # special character doesn't work

ansible host1 -m shell -a "echo welcome > /backup/file1"   #  it will work


ansible host1 -m command -a "rm -fr /backup/*"   # special character doesn't work
ansible host1 -m shell -a "rm -fr /backup/*"    # use shell module 

ansible host1 -m command -a "ls -lrth /root" 
```


##  Copy Module 

### Mode of Copy   
- ControlNode to ManageHost (copy module)
- ManageHost1 to ManageHost1  (copy module)    
- ManageHost to ControlNode (fetch module)
- ManageHost1 to ManageHost2
  - ManageHost1 to ControlNode (fetch module)
  - ControlNode to ManageHost2 (copy moduke)


#### Uses of src, dest

```bash
ansible host1 -m copy -a "content=welcome dest=/opt/content.demo"
ansible host1 -m copy -a "content='welcome to LP' dest=/opt/content.demo"

ansible host1 -m copy -a "src=/home/devuser1/copy.txt dest=/opt/"
ansible host1 -m copy -a "src=/home/devuser1/copy.txt dest=/opt/copy2.txt"
ansible host1 -m copy -a "src=/home/devuser1/copy.txt dest=/opt/copy3.txt owner=devops group=wheel mode=666"
```

#### Copy from `controlNode` to `host1`

```bash
ansible host1 -m copy -a "src=/dhaka dest=/opt/"

ansible host1 -m copy -a "src=/dhaka/file1 dest=/root/backup/"  # If the backup directory does not exist, it will be created.

ansible host1 -m copy -a "src=/dhaka/file1 dest=/root/backup" # If the backup directory does not exist, file1 will be saved as a backup.
```

#### Copy from `host1` to `host1` (`remote_src=yes`)

```bash
ansible host1 -m copy -a "src=/root/dhaka/file1 dest=/opt/ remote_src=yes"
```

#### Copy from `host1` to `ControlNode` (`fetch module`)

```bash
ansible host1 -m fetch -a "src=/root/backup/file1 dest=./"

ansible host1 -m fetch -a "src=/etc/hosts dest=./"  # copy with directory structure

ansible all -m fetch -a "src=/etc/hosts dest=./"
ls -l host1, host2

ansible host1 -m fetch -a "src=/etc/hosts dest=./ flat=yes"  # Only file will be copy.
ansible all -m fetch -a "src=/etc/hosts dest=./ flat=yes"  # Firstly copy from host1, the overwrite by host2.
```

#### Copy from `host1` to `host2`

```bash
ansible host1 -m fetch -a "src=/etc/hosts dest=./"   # Copy from host1 to ControlNode
ansible host2 -m copy -a "src=/home/devuser1/myproject/host1 dest=/backup/"  # Copy from ControlNode to host2
``` 

## Shell Module

```bash
ansible host1 -m shell -a 'echo $HOSTNAME'
ansible host1 -m shell -a "echo welcome > /backup/file1"
ansible host1 -m shell -a "echo $HOSTNAME >> /backup/file1"
```

## File Module 

The `file` module in Ansible is used to manage files, directories, and symbolic links on remote hosts. You can use it to create, delete, or modify file permissions.

- create file/dir
- delete file/dir
- change/modify file permission
- change/modify owner/group ownership
- create soft/hard link
- delete soft/hard link
- TimeStampUpdate

**Arguments in File Module:** 
- path: In the Ansible file module, the path argument specifies the file or directory on which the operation should be performed. Ex: /opt/file1
- src: The src argument in the Ansible file module is used when setting `symbolic links`. It defines the target path to which the symlink should point. (exact destinatin)
- dest: (Where link will be created)
- group
- mode: 777, u+r
- recurce: yes, no
- state
 - touch: create a blank file
 - directory: create a directory
 - absent: The absent argument in the Ansible file module is used with the state parameter to `remove` files, directories, or symlinks.
 - link: Soft Link
 - hard: Hard Link 

```bash
ansible-doc file
```

#### Create and delete files and directories.

```bash
ansible host1 -m file -a "path=/backup state=directory"  # Create a directory /backup
ansible host1 -m file -a "path=/backup/file1 state=touch"  # Create a file under /backup directory

ansible host1 -m file -a "path=/backup/file1 state=absent"  # Delete file1
ansible host1 -m file -a "path=/backup state=absent"  # Delete backup directory
# watch ls -ld /backups # from host1
```

#### Soft Link, Hard Link 

```
# Soft Link
ansible host1 -m file -a "src=/backup/file1 dest=/opt/file1 state=link"  # Soft Link Create
ansible host1 -m file -a "path=/opt/file1 state=absent"  # Link Delete

# Hard Link
ansible host1 -m file -a "src=/backup/file1 dest=/opt/file1 state=hard"
```

#### Uses of Mode

```bash
ansible host1 -m file -a "path=/backup state=directory owner=devops group=wheel mode=666
ansible host1 -m file -a "path=/backup state=directory owner=devops group=wheel mode=u+rw"
```


## `yum_repository` Module

The `yum_repository module` in Ansible is used to manage YUM repositories on RHEL-based systems (Red Hat, CentOS, Rocky Linux, AlmaLinux, etc.). It allows you to add, update, or remove repositories.

```
ansible host1 -m yum_repository -a "name=AppStream description='Packages from AppStream' baseurl=file:///software/AppStream"

# repo delete 
ansible host1 -m yum_repository -a "name=AppStream description='Packages from AppStream' baseurl=file:///software/AppStream state=absent" 

# AppStream
ansible host1 -m yum_repository -a "file=cdrom name=AppStream description='Packages from AppStream' baseurl=file:///software/AppStream gpgcheck=1 gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release enabled=1"

# BaseOS
ansible host1 -m yum_repository -a "file=cdrom name=BaseOS description='Packages from BaseOS' baseurl=file:///software/BaseOS gpgcheck=1 gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release enabled=1"
```

## Yum Module & Service Module 

The `yum` module in Ansible is used to manage packages on RHEL-based systems (Red Hat, CentOS, Rocky Linux, AlmaLinux, etc.). It allows you to install, remove, and update packages using YUM.

The `service` module in Ansible is used to manage services on Linux systems. It allows you to start, stop, restart, enable, or disable services, making it essential for system administration tasks.

- I - Install 
- S - Start
- E - Enable
- F - Firewall/iptables/ufw
- T - Test


  name: package <br/>
  state: present <br/>
         absent <br>
         latest <br>

#### Install Package
```
ansible all -m yum -a "name=httpd state=latest"
ansible host1 -m command -a "systemctl status httpd"
ansible all -m service -a "name=httpd state=started enabled=yes"

ansible all -m command -a "systemctl status httpd"  #check

ansible host1 -m service -a "name=httpd state=stopped"
ansible host1 -m service -a "name=httpd state=restarted"



ansible-doc -l | grep firewalld

ansible all -m command -a "firewall-cmd --permanent --add-service=http"
ansible all -m command -a "firewall-cmd --reload"


ansible all -m copy -a  "src=/var/www/html/index.html dest=/var/www/html/"

[devuser1@controlnode myproject]$ curl 192.168.11.101
```

```
ansible-doc service
```
#### Remove Package

#### Service 

## Firewall
#### Search on web `download ansible posix collection`
```
ansible --version
ansible-galaxy collection install ansible.posix
ansible --version

cd /home/devuser1/.ansible/collections/

/home/devuser1/.ansible/collections/ansible_collections/ansible/posix/plugins

ansible-doc firewalld
```

```
ansible host1 -m firewalld -a "service=ftp state=enabled"
ansible host1 -m firewalld -a "service=ftp state=enabled permanent=yes"
ansible host1 -m firewalld -a "service=ftp state=enabled permanent=yes immediate=yes"

ansible host1 -m firewalld -a "service=ftp state=disabled"


ansible host1 -m firewalld -a "port=29/tcp state=enabled immediate=yes"
ansible host1 -m firewalld -a "port=40-50/tcp state=enabled immediate=yes"


ansible host1 -a "firewall-cmd --list-all"

```



