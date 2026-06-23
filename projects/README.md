# `projects/`

End-to-end, real-world projects that combine multiple roles, playbooks,
templates, and variables into a working solution. Each project lives in its
own subdirectory and is **self-contained** — you should be able to `cd` in and
run it.

## Suggested project layout

```
projects/
└── <project-name>/
    ├── README.md          # what it does, prerequisites, how to run
    ├── site.yml           # entry-point playbook
    ├── inventory.ini      # or symlink to ../inventory.ini
    ├── group_vars/
    ├── host_vars/
    ├── playbooks/
    └── roles/
```

## Roadmap of projects (to be added)

1. **LAMP stack** — Linux + Apache + MySQL + PHP on Ubuntu
2. **Node.js app deployment** — systemd + nginx reverse proxy + logrotate
3. **WordPress on AWS** — EC2 + RDS + S3 + CloudFront
4. **Kubernetes bootstrap** — kubeadm + Calico + MetalLB
5. **Hardening baseline** — CIS-style SSH, firewall, auditd
6. **CI/CD runner** — self-hosted GitHub Actions runner with Docker
7. **Monitoring stack** — Prometheus + Grafana + node_exporter

Add new projects at the same level — one folder per project, with its own
README, so each can be reasoned about independently.
