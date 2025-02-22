# Overview of Ansible
- In one liner "Ansible is a super-simple automation platform that is agentless and extensible."
- It is an open source tool (with enterprise editions available) developed using Python and runs on linux, Mac, and UNIX-like systems.


## Ansible Architecture
- Control node
- Managed nodes
- Inventory
- Modules
- Tasks Playbooks

## Control node
- Any machine with Ansible installed.
- You can run commands and playbooks, invoking `/usr/bin/ansible or
/usr/bin/ansible-playbook`, from any control node.
- You can use any computer that has Python installed on it as a control node - laptops, shared desktops, and servers can all run Ansible.
- However, you cannot use a Windows machine as a control node.
- You can have multiple control nodes.
- Controller node refers this ansible.cfg to connect and work with ansible client nodes.
- Ansible will use SSH to connect to all the remote servers and executes the tasks in parallel


## Managed nodes
- The network devices (and/or servers) you manage with Ansible.
- Managed nodes are also sometimes called `hosts`.
- Ansible is not installed on managed nodes.
- Since we are not installing any agent or additional software on the client nodes, ansible is referred as agent less



## Inventory
- A list of managed nodes.
- An inventory file is also sometimes called a `hostfile `.
- Your inventory can specify information like IP address for each managed node.
- An inventory can also organize managed nodes, creating and nesting groups for easier scaling.



## Modules
- Ansible modules are reusable, standalone scripts that can be used by the Ansible API, or by the ansible or ansible-playbook programs
- Ansible ships with a number of modules (called the module library) that can be executed directly on remote hosts or through playbooks.
- Tasks in playbooks call modules to do the work.
- Each module has a particular use, from administering users on a specific type of database to managing VLAN interfaces on a specific type of network device.
- You can invoke a single module with a task, or invoke several different modules in a
playbook. For an idea of how many modules Ansible includes, take a look at the list of all modules.



## Tasks
A task in Ansible is a single action that gets executed on a managed node. Tasks are defined within a playbook and use Ansible modules to perform operations like installing packages, copying files, managing services, or running commands.
- The units of action in Ansible.
- You can execute a single task once with an `ad-hoc` command.

#### Example of task
```yml
#  Install a Package (YUM)
- name: Install Apache
  ansible.builtin.yum:
    name: httpd
    state: present

# Start a Service
- name: Start Apache
  ansible.builtin.service:
    name: httpd
    state: started
    enabled: yes

# Execute a Command
- name: Print hostname
  ansible.builtin.command: hostname


```
 
## Playbooks
An Ansible Playbook is a YAML file that defines a set of automation tasks to be executed on managed nodes. It describes what needs to be done, such as installing packages, configuring services, deploying applications, or managing users. Playbooks use plays, which group tasks and define the hosts they apply to.

An Ansible playbook contains one or multiple plays, each of which define the work to be done for a configuration on a managed server. Ansible plays are written in YAML. Every play is created by an administrator with environment-specific parameters for the target machines; there are no standard plays.

- Ordered lists of tasks, saved so you can run those tasks in that order repeatedly.
- Playbooks can include variables as well as tasks.
- Playbooks are written in YAML and are easy to read, write, share and understand.

#### Example 

```yml
---
- name: Install and start Apache
  hosts: webservers
  become: yes  # Run tasks with sudo privileges
  tasks:
    - name: Install Apache
      ansible.builtin.yum:
        name: httpd
        state: present

    - name: Start Apache
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: yes

```

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
We can also add a collection of hosts in `webservers` group
```bash
[webservers]
host1
host2
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

Check host from inventory file 
```bash
ansible all --list-hosts  # Check all host from inventory file 
ansible '*' --list-hosts  # Check all host from inventory file 
ansible-inventory --list  # Check all host from inventory file 

ansible host1 --list-hosts       # Check single host from inventory file
ansible webservers --list-hosts  # Check host from inventory file by group name
ansible ungrouped --list-hosts   # Check ungrouped host from inventory file
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

By default, Ansible tries key-based authentication. If `PermitRootLogin no` is set on managed nodes, Ansible will not be able to log in as root via SSH.
```bash
vi /etc/ssh/sshd_config.d/01-permitrootlogin.conf
    PermitRootLogin no
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
ansible all -a hwclock -u devops -k # if devops user has not sudo privileged, this command will not be executed
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


