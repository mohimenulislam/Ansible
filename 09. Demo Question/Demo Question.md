
### 1. Install and configure Ansible 
#### Install & config Ansible on the control node workstation
- install the require packages 
- create a static file called `/home/devops/ansible/inventory` so that: 
  - **servera** is a member of the dev host group 
  - **serverb** is a member of the test host group 
  - **serverc** and **serverd** are members of the **prod host group** 
  - **bastion** is a member of the **balancers host group**
  - The **prod** group is Members of the **webservers host group** 
#### Create a configuration file called /home/devops/ansible/ansible.cfg**
 - The **host inventory** file is `/home/devops/ansible/inventory`
 - The **location** of **role** used in **playbooks** include `/home/devops/ansible/roles`



After Local Login: `/etc/motd`
Before Login: `/etc/issue` >> Screen Dsplay
