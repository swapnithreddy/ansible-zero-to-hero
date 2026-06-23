# `handlers/`

Handlers are tasks that run **only when notified** by another task (typically
when something has changed). They are most commonly used to restart services
after their configuration has been updated.

## When to use a handler

- Restart `nginx` after editing `nginx.conf`
- Reload `ufw` after firewall rules change
- Restart `postgresql` after `postgresql.conf` is rewritten

## Example

In a task:

```yaml
- name: Deploy nginx site config
  ansible.builtin.template:
    src: nginx.conf.j2
    dest: /etc/nginx/sites-available/app.conf
  notify: restart nginx
```

In a handler (typically in `roles/<role>/handlers/main.yml` or here at the
playbook level):

```yaml
- name: restart nginx
  ansible.builtin.service:
    name: nginx
    state: restarted
```

## Convention used in this repo

Handlers live inside each role under `roles/<role>/handlers/main.yml`.
This top-level `handlers/` directory is reserved for **playbook-level handlers**
that don't belong to a single role.
