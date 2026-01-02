# Proxmox PVE Post-Install Setup

Ansible playbooks for configuring fresh Proxmox VE installations.

## Quick Install

```bash
curl -sSL https://raw.githubusercontent.com/JDeRose-net/proxmox-pve/master/install.sh | bash
```

Custom username (default: `sysadm`):

```bash
curl -sSL https://raw.githubusercontent.com/JDeRose-net/proxmox-pve/master/install.sh | NEWUSER=myuser bash
```

## After Install

As root:

```bash
cd /opt/proxmox-pve
ansible-playbook -i inventory/local.yml playbooks/site.yml
passwd sysadm
```

## Inventory Options

| Inventory | Connection | Use case |
|-----------|------------|----------|
| `local.yml` | local | Running on the host you're configuring |
| `local-dev.yml` | local | Local with dev tools (strace, tcpdump) |
| `remote-dev.yml` | SSH | Remote dev/test hosts |
| `remote-prod.yml` | SSH | Remote production (strict SSH, fail2ban) |

## What It Does

- Creates non-privileged user with sudo
- Disables enterprise repos, enables no-subscription repo
- Installs common packages (htop, git, vim, tmux, etc.)
- Configures SSH hardening
- Removes subscription nag from web UI

## Structure

```
inventory/
├── group_vars/  # Environment-specific variables
├── local.yml    # Run on this host
├── local-dev.yml
├── remote-dev.yml
└── remote-prod.yml
roles/           # base, users, security, proxmox
```
