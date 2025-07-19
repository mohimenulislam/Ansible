## Ansible Vault

Ansible Vault is a feature of Ansible that allows you to encrypt sensitive data such as:

### ansible vault
- create new encrypted file
- encrypt existing file
- password can be stored into file
```bash
ansible-vault --help
```

```bash
ansible-vault create sample.secret   # Created   ansible-vault, type something
cat sample.secret  # text will be encrypted 
ansible-vault view sample.secret  # View
```

#### Ansible vault pass saved other file 

Here vault password saved in `.secret.txt` 
```bash
echo pass321 > .secret.txt
chmod 600 .secret.txt
``` 
Here no password need to input
```bash
ansible-vault view sample.secret --vault-password-file=./.secret.txt
```

Or (paassword take from `.secret.txt` while creating the ansible vault
```bash
echo pass321 > .secret.txt
chmod 600 .secret.txt
ansible-vault create sample.secret --vault-password-file=./.secret.txt
```

#### Ansible vault edit

```bash
ansible-vault edit sample.secret # (required password)
# or
ansible-vault edit sample.secret --vault-password-file=./.secret.txt #(no password required)
```

#### Vault Password change

```bash
ansible-vault rekey sample.secret
```

#### Ansible vault Encrypt & Decryptx existing file

**Encrypt**
```bash
ansible-vault encrypt existingfile.yaml    # write something
cat existingfile.yaml
```

**Dry run**
```bash
ansible-playbook multiuser1.yaml -C  # Showing error
ansible-playbook multiuser1.yaml -C --ask-vault-pass
```
We can also define password in another file
```bash
echo pass321 > pass.txt
ansible-playbook multiuser1.yaml -C --vault-password-file=pass.txt
```


**Decrypt**
```bash
ansible-vault decrypt existingfile.yaml
```


#### IF playbook variable file has encrypted file

Create a file name `welcome.txt`
```yaml
welcome: Welcome to ansible
# encrypt the file 
ansible-vault encrypt welcome.txt 
```

#### Create a file name `welcomeprint.yaml` and its password save to `pass.txt` file
```yaml
---
- name: Print Welcome msg
  hosts: servera
  vars_files:
   - ./welcome.txt     ## encrypted file
  tasks:
   - name: Wellcome msg
     debug:
       msg: "{{ welcome }}"
```

#### If both file (welcomeprint.yaml, welcome.txt) password is same the it will be run
```bash
ansible-playbook welcomeprint.yaml -C --vault-password-file=pass.txt
```

#### If both password is not same then 

We have to create another password file `welcomepass.txt` for `welcome.txt`

```bash
ansible-playbook multiuser22.yaml -C --vault-password-file=pass.txt --vault-password-file=welcomepass.txt
```


#### We mention the password file in configuration file & don't mention  `--vault-password-file`

```yaml
[defaults]
inventory=/home/devops/ansible/inventory
remote_user=devops
vault_password_file=./pass.txt    # new

[privilege_escalation]
become=true
mecome_method=sudo
become_user=root
become_ask_pass=false
```
Here file (welcomeprint.yaml, welcome.txt) both password have to same
```
ansible-playbook welcomeprint.yaml -C 
```
