# `roles/`

Roles are Ansible's unit of reusable automation. A role bundles **tasks**,
**handlers**, **templates**, **files**, **variables**, and **defaults** into a
self-contained, named package that any playbook can invoke.

## Standard role layout

```
roles/
└── <role_name>/
    ├── README.md           # what this role does, inputs, outputs
    ├── defaults/
    │   └── main.yml        # overridable default variables
    ├── vars/
    │   └── main.yml        # internal variables (higher precedence)
    ├── tasks/
    │   └── main.yml        # the work
    ├── handlers/
    │   └── main.yml        # restart/reload handlers
    ├── templates/          # *.j2 files
    ├── files/              # static files copied as-is
    ├── meta/
    │   └── main.yml        # role dependencies, galaxy metadata
    └── tests/
        ├── test.yml        # molecule / sanity playbook
        └── inventory.ini
```

## Create the skeleton quickly

```bash
ansible-galaxy init roles/<role_name>
```

## Use a role in a playbook

```yaml
- hosts: webservers
  roles:
    - common
    - nginx
    - role: app
      vars:
        app_port: 8080
```

## Convention used in this repo

Each role has its own folder and its own README. Roles stay **single-purpose**
(one role = one well-defined job) and avoid cross-role variable dependencies
except through `meta/main.yml` dependencies, which are declared explicitly.
