# `apache`

Installs and configures the Apache HTTP Server on Debian/Ubuntu hosts.

## What it does

- Installs the `apache2` package
- Deploys a static landing page to `/var/www/html/index.html`
- Ensures the `apache2` service is running and enabled at boot

## Variables

| Name                | Default       | Description                              |
|---------------------|---------------|------------------------------------------|
| `apache_index_src`  | `index.html`  | Source file inside the role's `files/`   |
| `apache_index_dest` | `/var/www/html/index.html` | Destination path on the host |

## Example

```yaml
- hosts: webservers
  become: true
  roles:
    - apache
```
