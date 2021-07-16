# Prepare playbook

Create a playbook file
```shell
touch playbook.yml
```

Let's rewrite our adhoc command to the playbook format:

```yaml
- hosts: all

  tasks:
    - name: Copy file
      copy:
        src: test.txt
        dest: /tmp/text.txt
```

Then let's play with some other modules (copy, file, apt, git):

```yaml
- hosts: all

  tasks:
    - name: Copy file
      copy:
        src: test.txt
        dest: /tmp/text.txt

    - name: Create a directory if it does not exist
      file:
        path: /etc/some_directory
        state: directory
        mode: '0755'

    - name: Install git
      apt:
        package: git
        state: present

    - name: Clone repo
      git:
        repo: https://github.com/codex-team/workflow.git
        dest: ~/workflow
```

# Run playbook

You can run playbook with
```
ansible-playbook playbook.yml
```

ansible.cfg will help us not to specify `.hosts` file as argument.
```
[defaults]
inventory = ./hosts
```
