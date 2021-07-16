# General instructions

To install galaxy role
```bash
ansible-galaxy install <role_name>
```

For example `ansible-galaxy install geerlingguy.docker`

## requirements.yml

All the requirements can be installed from a special yml file.
```
- name: geerlingguy.docker
``` 

with the following command
```
ansible-galaxy install -r <file_name>
```

## Playbook

Now we can use this role as usual to install Docker
```
---

- hosts: ourhost

  roles:
    - name: geerlingguy.docker
```

and run with a command
```
ansible-playbook docker-playbook.yml
```
