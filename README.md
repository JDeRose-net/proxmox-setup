# Proxmox Post-Install Setup

Ansible playbooks for configuring fresh Proxmox installations (PVE, PBS, PMG).

## Quick Install

```bash
curl -sSL https://raw.githubusercontent.com/JDeRose-net/proxmox-setup/master/install.sh | bash
```

Custom username (default: `sysadm`):

```bash
curl -sSL https://raw.githubusercontent.com/JDeRose-net/proxmox-setup/master/install.sh | NEWUSER=myuser bash
```

## After Install

As root:

```bash
cd /opt/proxmox-setup
ansible-playbook -i inventory/local.yml playbooks/site.yml -e local_user=sysadm
passwd sysadm
```

## Playbooks

| Playbook | Description |
|----------|-------------|
| `site.yml` | Full setup (imports pve-setup + user) |
| `pve-setup.yml` | Core PVE config (packages, SSH, subscription nag) |
| `user.yml` | User management only (create user, sudo, SSH keys) |

Run individually:

```bash
# Just PVE config (no user)
ansible-playbook -i inventory/local.yml playbooks/pve-setup.yml

# Just user setup
ansible-playbook -i inventory/local.yml playbooks/user.yml -e local_user=sysadm
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
playbooks/
├── site.yml       # Full setup
├── pve-setup.yml  # Core PVE config
└── user.yml       # User management
inventory/
├── group_vars/    # Environment-specific variables
├── local.yml
├── local-dev.yml
├── remote-dev.yml
└── remote-prod.yml
roles/
├── base           # Packages, timezone
├── users          # User creation, sudo, SSH keys
├── security       # SSH hardening, fail2ban
└── proxmox        # PVE-specific (subscription nag)
```
