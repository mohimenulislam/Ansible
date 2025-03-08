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


backup, content, follow, mode, owner,

```bash
ansible host1 -m copy -a "content=welcome dest=/opt/content.demo"
ansible host1 -m copy -a "content='welcome to LP' dest=/opt/content.demo"

ansible host1 -m copy -a "src=/home/devuser1/copy.txt dest=/opt/"
ansible host1 -m copy -a "src=/home/devuser1/copy.txt dest=/opt/copy2.txt"
ansible host1 -m copy -a "src=/home/devuser1/copy.txt dest=/opt/copy3.txt owner=devops group=wheel mode=666"
```


