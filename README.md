# ansible-zero-to-hero

🚀 Master Ansible through hands-on labs, real-world automation projects, and
production-ready examples — from beginner concepts to advanced DevOps
workflows.

---

## What this repo is

A structured, opinionated learning path for Ansible. Each lesson is a small,
runnable piece of automation that builds on the previous one. Topics progress
from "what is an inventory?" to "deploy a full multi-tier application with
roles, templates, and Vault-encrypted secrets."

It is **not** a copy of the official Ansible docs. It is a working lab: every
playbook in this repo is meant to be run, edited, and broken on purpose.

## Repository layout

```
ansible-zero-to-hero/
├── ansible.cfg                  # project-wide Ansible defaults
├── inventory.ini                # your hosts (replace with your own)
├── inventory.example.ini        # documented template
├── README.md                    # you are here
├── LICENSE
├── .gitignore
│
├── playbooks/                   # entry-point playbooks
│   ├── apache_site.yml          # canonical role-based example
│   └── first_playbook/          # minimal "everything inline" example
│       ├── first_playbook.yaml
│       └── index.html
│
├── roles/                       # reusable roles
│   └── apache/                  # install Apache + deploy a static page
│       ├── README.md
│       ├── defaults/main.yml
│       ├── tasks/main.yml
│       ├── handlers/main.yml
│       ├── meta/main.yml
│       └── files/index.html
│
├── group_vars/                  # variables applied to inventory groups
│   └── all.yml
│
├── handlers/                    # playbook-level handlers (see ./handlers/README.md)
├── templates/                   # playbook-level templates (see ./templates/README.md)
├── vars/                        # playbook-level variables (see ./vars/README.md)
└── projects/                    # end-to-end, real-world projects (see ./projects/README.md)
```

Every directory has its own README explaining its purpose and conventions.

## Prerequisites

- A control machine (Linux or macOS) with **Ansible 2.12+**
  - `brew install ansible` on macOS
  - `pip install --user ansible-core` or `pipx install ansible-core` on Linux
- Python 3.8+
- One or more target hosts reachable over SSH
  - EC2, DigitalOcean, GCP, or local VMs all work
  - Ubuntu/Debian is assumed for `apt`; extend with roles for other distros
- An SSH key already authorized on your target hosts

Verify your install:

```bash
ansible --version
```

## Quick start

```bash
# 1. Clone and enter the repo
git clone git@github.com:swapnithreddy/ansible-zero-to-hero.git
cd ansible-zero-to-hero

# 2. Configure your hosts
cp inventory.example.ini inventory.ini
$EDITOR inventory.ini

# 3. Confirm Ansible can reach them
ansible all -m ping

# 4. Run the role-based example
ansible-playbook playbooks/apache_site.yml

# 5. Run the inline example (same outcome, different style)
ansible-playbook playbooks/first_playbook/first_playbook.yaml
```

After step 4 or 5, open `http://<your-host>/` in a browser — you should see
the Ansible Learning Hub landing page.

## Learning roadmap

Progress through these in order. Each topic maps to a folder or example in
this repo.

| #  | Topic                              | Status   | Where to look                       |
|----|------------------------------------|----------|-------------------------------------|
| 1  | Installation & `ansible.cfg`       | ✅ Done  | `ansible.cfg`                       |
| 2  | Inventory files & groups           | ✅ Done  | `inventory.ini`, `inventory.example.ini` |
| 3  | Ad-hoc commands & modules          | ✅ Done  | `ansible all -m ping`               |
| 4  | Playbooks (inline tasks)           | ✅ Done  | `playbooks/first_playbook/`         |
| 5  | Roles & `roles_path`               | ✅ Done  | `roles/apache/`                     |
| 6  | Variables, defaults, group_vars    | ✅ Done  | `roles/apache/defaults/`, `group_vars/` |
| 7  | Handlers                           | ✅ Done  | `roles/apache/handlers/`            |
| 8  | Templates (Jinja2)                 | ✅ Done  | `templates/README.md`               |
| 9  | Files & `copy` module              | ✅ Done  | `roles/apache/tasks/main.yml`       |
| 10 | Ansible Vault for secrets          | 🔜 Next  | `vars/`                             |
| 11 | Loops, conditionals, register      | 🔜 Next  |                                     |
| 12 | Dynamic inventory                  | 🔜 Next  |                                     |
| 13 | Collections & Galaxy               | 🔜 Next  |                                     |
| 14 | Real-world projects                | 🔜 Next  | `projects/`                         |

## Common commands

```bash
# Check syntax without running anything
ansible-playbook --syntax-check playbooks/apache_site.yml

# Dry run (see what would change, change nothing)
ansible-playbook --check playbooks/apache_site.yml

# Run with extra verbosity
ansible-playbook -v playbooks/apache_site.yml

# Limit to a subset of hosts
ansible-playbook --limit server1 playbooks/apache_site.yml

# Encrypt a sensitive variables file
ansible-vault encrypt vars/secrets.yml

# Run a playbook using a vault-encrypted file
ansible-playbook --ask-vault-pass playbooks/apache_site.yml

# Show facts gathered about a host
ansible server1 -m setup
```

## Conventions used in this repo

- **YAML files use `.yml`** (not `.yaml`) for everything except the legacy
  `first_playbook.yaml`, which is preserved as the "inline style" example.
- **Roles are single-purpose.** One role = one well-defined job. Cross-role
  variable sharing goes through `meta/main.yml` dependencies or explicit
  `vars_files`, never through magic imports.
- **Templates end in `.j2`** so the rendering pipeline is obvious.
- **Secrets are never committed.** Untracked `*vault*.yml` and `*secrets*.yml`
  files are ignored by `.gitignore`. Use `ansible-vault` for the real thing.
- **The `inventory.ini` is yours.** Treat it as personal config; the template
  is in `inventory.example.ini`.

## Extending the lab

Suggested next additions (PRs welcome):

1. **A `common` role** — baseline hardening (timezone, NTP, fail2ban, unattended-upgrades)
2. **A `nginx` role** — mirror of `apache` for the reverse-proxy case
3. **A `nodejs_app` role** — systemd unit + nginx reverse proxy + logrotate
4. **A `monitoring` role** — Prometheus node_exporter + Grafana
5. **A multi-host playbook** under `projects/` that ties several roles together
6. **Molecule tests** under each role's `tests/`

See [`projects/README.md`](./projects/README.md) for the long-term roadmap.

## Contributing

1. Fork the repo and create a topic branch (`feat/<role-name>` or `fix/<thing>`)
2. Keep role changes self-contained — touch one role per commit where possible
3. Run `ansible-playbook --syntax-check` on anything you change
4. Open a PR describing what was added and which roadmap step it advances

## License

MIT — see [`LICENSE`](./LICENSE).
