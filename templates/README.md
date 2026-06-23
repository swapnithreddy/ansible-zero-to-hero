# `templates/`

Templates are **Jinja2-templated files** that Ansible renders and deploys to
target hosts. Use them whenever the destination file needs variables,
conditionals, or loops substituted in at runtime.

## When to use a template

- Config files that vary per host (`bind_address`, `node_name`, secrets)
- Service configs driven by inventory groups
- Any file that needs `{% if %}`, `{% for %}`, or filters

## File extension

Always use `.j2` for templates so the rendering pipeline is obvious:

```
templates/
└── nginx.conf.j2
```

## Example

`templates/app.conf.j2`

```jinja2
server {
    listen {{ nginx_port | default(80) }};
    server_name {{ inventory_hostname }};

    {% for upstream in app_upstreams %}
    upstream {{ upstream.name }} {
        server {{ upstream.host }}:{{ upstream.port }};
    }
    {% endfor %}
}
```

In a playbook:

```yaml
- name: Deploy app config
  ansible.builtin.template:
    src: app.conf.j2
    dest: /etc/nginx/conf.d/app.conf
    owner: root
    group: root
    mode: '0644'
```

## Convention used in this repo

Role-specific templates live inside the role at `roles/<role>/templates/`.
This top-level `templates/` directory is reserved for **playbook-level
templates** shared across roles.
