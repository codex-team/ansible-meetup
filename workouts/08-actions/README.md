# General instructions

Ansible roles can be deployed via GitHub Actions.
We hold all the sensitive information in GitHub secrets and run playbooks.

For now we can use publicly available GitHub Action for Ansible - https://github.com/dawidd6/action-ansible-playbook

## Initialization

Let's create a GitHub action using `dawidd6/action-ansible-playbook@v2`.
We pass the following parameters:

- `playbook` - name of a playbook file to run
- `directory` - working directory where the playbook file is
- `key` - SSH private key (stored in GitHub secrets)
- `inventory` - hosts description
- `requirements` - Ansible Galaxy requirements

Do not forget to add `IP_ADDRESS` and `SSH_PRIVATE_KEY` as secrets https://docs.github.com/en/actions/reference/encrypted-secrets

```
name: deploy ansible

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v1
      - name: Run playbook
        uses: dawidd6/action-ansible-playbook@v2
        with:
          playbook: docs.yml
          directory: ./ansible
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          inventory: ourhost ansible_host=${{ secrets.IP_ADDRESS }} ansible_user=root
          requirements: requirements.yml

```

## Writing Ansible playbook

Let's add playbook and requirements files to the `ansible` directory.

docs.yml
```
---

- hosts: ourhost

  roles:
    - name: n0str.ansible_role_codex_docs
      vars:
        listen_interface: "0.0.0.0"
        listen_port: 7071
```

requirements.yml
```
# from galaxy
- name: n0str.ansible_role_codex_docs
```

## Running

Push changes to the master branch and see results in Actions section of the GitHub repository page.
