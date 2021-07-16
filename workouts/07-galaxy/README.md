# General instructions

To publish your Ansible Role to the Ansible Galaxy, you need to pass the following steps:
   
- initialize directory structure for Ansible Role
- setup `meta` data for the role
- Register on Ansible Galaxy website and get API token
- Publish your role as GitHub repository
- Import your repository to the Galaxy with CLI command

## Initialization

Using the `ansible-galaxy` command line tool that comes bundled with Ansible, 
you can create a role with the init command. 

Let's create codex-docs role directory structure:
```
ansible-galaxy init codex-docs
```

## Edit metadata
Now you can edit role metadata in the `codex-docs/meta/main.yml` file:
```
galaxy_info:
  author: N0str
  description: Ansible role for codex-docs documentation engine installation
  company: CodeX Team
  license: MIT
  min_ansible_version: 2.1

  platforms:
    - name: Ubuntu
      versions:
        - bionic
        - xenial
        - focal

  galaxy_tags:
    - codex
    - documentation
    - development

dependencies: []
```

## Registration and Login

1. Register via GitHub on https://galaxy.ansible.com/login
2. Take your personal API key from https://galaxy.ansible.com/me/preferences

## Publish GitHub repository

You can `git init` the repository and push it as usually to the GitHub.
See [instructions](https://docs.github.com/en/github/importing-your-projects-to-github/importing-source-code-to-github/adding-an-existing-project-to-github-using-the-command-line)

## Import your repository

You can push your repository to the Ansible Galaxy with `ansible-galaxy role import` command.
```
ansible-galaxy role import --token <YOUR_API_TOKEN> <GITHUB_USER> <GITHUB_REPO>
```

where:

- YOUR_API_TOKEN - token that you've got on Ansible Galaxy settings page
- GITHUB_USER - your username (ex: `n0str`)
- GITHUB_REPO - your repository name (ex: `ansible_role_codex_docs`)

Now your repository https://github.com/n0str/ansible_role_codex_docs will be linked with
Ansible Galaxy role https://galaxy.ansible.com/n0str/ansible_role_codex_docs

# Run and use

Now you can use this role in your playbook.
```
ansible-galaxy install -r requirements.yml
ansible-playbook -i hosts docs-playbook-prod.yml
```

Where `requirements.yml` consists of your role
```
# from galaxy
- name: n0str.ansible_role_codex_docs
```

and `docs-playbook-prod.yml` which uses the role
```
---

- hosts: ourhost

  roles:
    - name: n0str.ansible_role_codex_docs
      vars:
        listen_interface: "0.0.0.0"
        listen_port: 7070
```


# Troubleshooting

There can be several difficulties

## SSL: CERTIFICATE_VERIFY_FAILED

The problem can occure when your Python environment is not finding/making use of the default root certificates that 
are installed on your OS. 
These root certs are required to connect securely (via TLS) with Ansible Galaxy.

Usually can happen on MacOS.
You can add `--ignore-certs` flag to the `ansible-galaxy` command to address the problem.

More details: https://stackoverflow.com/questions/63534262/how-to-fix-following-ansible-galaxy-ssl-error

