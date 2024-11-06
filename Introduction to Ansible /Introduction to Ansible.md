## Introduction to Ansible

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
- `ansible-playbook`
- `ansible-vault`
- `ansible-config`
- `ansible-connection`
- `ansible-console`
- `ansible-doc`
- `ansible-galaxy`
- `ansible-inventory`
- `ansible-pull`

> [!NOTE]
> - Ansible is agentless, Only control node have to installed python<br>
> - Ansible can mange - Windows, cisco router, firewall, cloud infrastructure.<br>
> - `Control node`: The machine from which Ansible CLI tools are run. 
> - `Managed nodes`: The nodes that receive commands and instructions from the control node.
> - `Playbooks`: Playbooks are YAML files that orchestrate multiple tasks to automate complex processes.
> - `Task`: A task is the smallest unit of action you can automate using an Ansible playbook.
> - `Inventory`: An inventory in Ansible is a list of hosts and groups of hosts that Ansible uses to run commands, modules, and tasks.
  


