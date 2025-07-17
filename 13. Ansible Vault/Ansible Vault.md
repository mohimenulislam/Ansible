## Ansible Vault

Ansible Vault is a feature of Ansible that allows you to encrypt sensitive data such as:

### ansible vault
- create new encrypted file
- encrypt existing file
- password can be stored into file

```bash
ansible-vault create sample.secret # Create ansible-vault, type something
cat sample.secret  # text will be encrypted 
ansible-vault view sample.secret  # View
```

#### Ansible vault pass saved other file 

Here vault password saved in `.secret.txt` 
```
echo pass321 > .secret.txt
chmod 600 .secret.txt
```
Here no password need to input
```
ansible-vault view sample.secret --vault-password-file=./.secret.txt
```

Or (paassword take from `.secret.txt` while creating the ansible vault
```
echo pass321 > .secret.txt
chmod 600 .secret.txt
ansible-vault create sample.secret --vault-password-file=./.secret.txt
```

#### Ansible vault modify 

```
ansible-vault edit sample.secret # (required password)
# or
ansible-vault edit sample.secret --vault-password-file=./.secret.txt #(no password required)
```
