## Getting started with Ansible
Ansible automates the management of remote systems and controls their desired state. Ansible environments have three main components:

- `Control node`: A system on which Ansible is installed. You run Ansible commands such as ansible or ansible-inventory on a control node.

- `Inventory`: A list of managed nodes that are logically organized. You create an inventory on the control node to describe host deployments to Ansible.

- `Managed node`: A remote system, or host, that Ansible controls

Ansible provides open-source automation that reduces complexity and runs everywhere. Using Ansible lets you automate virtually any task. Here are some common use cases for Ansible:

- Eliminate repetition and simplify workflows
- Manage and maintain system configuration
- Continuously deploy complex software
- Perform zero-downtime rolling updates

Ansible uses simple, human-readable scripts called playbooks to automate your tasks. You declare the desired state of a local or remote system in your playbook. Ansible ensures that the system remains in that state.

- `Agent-less architecture`: Low maintenance overhead by avoiding the installation of additional software across IT infrastructure.
- `Simplicity`: Automation playbooks use straightforward YAML syntax for code that reads like documentation. Ansible is also decentralized, using SSH with existing OS credentials to access to remote machines.

- `Scalability and flexibility`: Easily and quickly scale the systems you automate through a modular design that supports a large range of operating systems, cloud platforms, and network devices.

- `Idempotence and predictability`: When the system is in the state your playbook describes Ansible does not change anything, even if the playbook runs multiple times.



### Ansible Additional Commands

- `ansible`
- `ansible --version`
- `ansible-playbook`
- `ansible-vault`
- `ansible-config`
- `ansible-connection`
- `ansible-console`
- `ansible-doc`
- `ansible-galaxy`
- `ansible-inventory`
- `ansible-pull`
- [root@controlnode ~]# root [press tab] - To see available command 



> [!NOTE]
> - Ansible is agentless, Only control node have to installed python<br>
> - Ansible can mange - Windows, cisco router, firewall, cloud infrastructure.<br>
> - `Playbooks`: Playbooks are YAML files that orchestrate multiple tasks to automate complex processes.
> - `Task`: A task is the smallest unit of action you can automate using an Ansible playbook.


  
Ansible configuration file
```bash
rpm -qc ansible-core

/etc/ansible/ansible.cfg
/etc/ansible/hosts    #inventory file
```

> [!NOTE]
> We can place ansible configuration file in different four location. 


### Building an inventory
Add host in inventory file 
`vi /etc/ansible/hosts`
```bash
host1
host2
```

Check all host from inventory file 
```bash
ansible all --list-hosts
ansible '*' --list-hosts
ansible-inventory --list 
```
Check single host from inventory file
```bash
 ansible host1 --list-hosts
```

We can also add a collection of hosts in `webservers` group
```bash
[webservers]
host1
host2
```
Check host from inventory file by group name
```bash
ansible webservers --list-hosts
```

If we isolate host4 from group
```bash
host4

[webservers]
host1
host2

[dbservers]
host3
```

Check ungrouped host from inventory file 
```bash
ansible ungrouped --list-hosts
```


  
## Ansible Module
An Ansible module is a small program that performs actions on a local machine, application programming interface (API), or remote host. Modules are expressed as code, usually in Python, and contain metadata that defines when and where a specific automation task is executed and which users can execute it.

Plugin documentation tool
```bash
man ansible-doc
```

List of all ansible module
```bash
ansible-doc -l
```

Count all ansible module
```bash
ansible-doc -l | wc -l
```

### Ping Module 
```bash
ansible-doc ping
ansible all -m ping
    # -m: module
```

By default ansible try keybase authentication, 
if  PermitRootLogin is no
```bash
vi /etc/ssh/sshd_config.d/01-permitrootlogin.conf
    PermitRootLogin yes
```

Then, if we try for passwordbase authentication 
```bash
ansible --help
ansible all -m ping --ask-pass
ansible all -m ping -k
```

### Command Module
```bash
ansible all -m command -a date
#or
ansible all -a date # if there doesn't `-m command` it will be by default command module.
```

### For other user
We will create `devops` user at host1 and hos2.
Now we will try `devops` as remote user
```bash
ansible all -m ping -u devops  # if devops user has not sudo privileged, then it will not worked.
ansible all -m ping -u devops -k
ansible all -a date -u devops -k
ansible all -a hwclock -u devops -k # This command will not be executed
```

> [!NOTE]
> `date` command show system date and time, `hwclock` command show BIOS date and time & this command must be executed from sudo privileged user.

give `devops` user sudo privileged for `host1` and `host2`
```bash
vi /etc/sudoers.d/devops
  devops ALL=(ALL)  NOPASSWD: ALL
su - devops
sudo -l # check sudo privileged
```

Then, `hwclock` command will be execute
```bash
ansible all -a hwclock -u devops -k -b
    # -b: become: devops user become as a root
    # ansible --help
```

Now, if we use devops as default user
```bash
vi /etc/ansible/ansible.cfg

    [defaults]
    remote_user=devops
    [privilege_escalation]

ansible all -a hwclock -k -b 
```
Remove ask pass & ssh public id copy to devops user.
```bash
ssh-copy-id devops@host1
ssh-copy-id devops@host2

ansible all -a hwclock -b
```

Remove become
```bash
vi /etc/ansible/ansible.cfg

    [defaults]
    remote_user=devops
    [privilege_escalation]
    become=true
    become_user=root
    become_method=sudo
    become_ask_pass=false

ansible all -a hwclock
```


