
Group  >>   Primary (Private) <br>
            Secondary (Suplimentary)

A user must have a primary group.
user1 >> primary group 


#### User Module 
```
ansible-doc user
ansible-doc group
```

#### Create User
```
ansible servera -m user -a "name=mokles"
```

#### Remove user
```
ansible servera -m user -a "name=mokles state=absent"
```

#### Remove user with home directory
```
ansible servera -m user -a "name=mokles state=absent remove=yes"
```

#### Assign multiple valus
group->primary
groups->Secondary
```
ansible servera -m user -a "name=mokles state=present uid=1020  shell=/bin/sh comment=NetworkTeam groups=cisco"
```

#### Assign user to another group.
```
ansible servera -m user -a "name=mokes  groups=hpe append=yes" 
```

#### when we creat a user for system uses purpose, then must `system=true`. Therefore system is will be less the 1000



#### Create `cisco` group
```
ansible servera -m group -a "name=cisco state=present"
```

#### Delete `cisco` group
```
ansible servera -m group -a "name=cisco state=absent"
```


### Answer Question
