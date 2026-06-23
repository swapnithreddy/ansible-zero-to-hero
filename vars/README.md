# `vars/`

This directory holds **playbook-level variables** — values that need to be
defined in one place and reused across multiple tasks, plays, or included
playbooks.

## Difference between `vars/` and `vars_files` inside roles

| Location                          | Scope                                |
|-----------------------------------|--------------------------------------|
| `roles/<role>/vars/main.yml`      | Variables for **one role only**      |
| `roles/<role>/defaults/main.yml`  | Default values overridable by users  |
| `group_vars/<group>.yml`          | Variables for an inventory group     |
| `host_vars/<host>.yml`            | Variables for a single host          |
| `vars/` (this directory)          | Variables loaded by a playbook       |

## Conventions

- Use clear, lowercase, snake_case names: `app_port`, `db_password`.
- Never commit real secrets. Use [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/index.html)
  for encrypted files (e.g. `vars/production/vault.yml`).
- Prefer `defaults` over `vars` when the variable should be overridable.

## Example

`vars/common.yml`:

```yaml
---
app_name: my-web-app
app_user: www-data
app_port: 8080
nginx_worker_processes: auto
```

Load in a playbook:

```yaml
- name: Configure app
  hosts: webservers
  vars_files:
    - ../vars/common.yml
  roles:
    - nginx
    - app
```
