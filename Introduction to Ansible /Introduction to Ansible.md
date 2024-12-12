## Introduction to Ansible
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
