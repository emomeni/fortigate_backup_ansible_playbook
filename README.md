# FortiGate Backup Automation

Automated backup solution for FortiGate firewalls using Ansible.

## Watch the Video on YouTube
[![Watch the video](./images/fortigate_automation_2_thumbnail.jpg)](https://youtu.be/9uGvibNRikI?si=pZb78t-LjLSXdysn)


## Features

- ✅ Automated configuration backups
- ✅ Device information documentation
- ✅ Automatic retention management (configurable per device)
- ✅ Parallel execution for multiple devices
- ✅ Comprehensive error handling
- ✅ Audit logging
- ✅ Support for multiple device groups

## Quick Start

### 1. Install Prerequisites
```bash
pip3 install ansible fortiosapi
ansible-galaxy collection install -r requirements.yaml
```

### 2. Configure Credentials
```bash
export FORTIGATE_USER="admin"
export FORTIGATE_PASSWORD="YourPassword"
```

### 3. Update Inventory

Edit `hosts.yaml` and update IP addresses for your FortiGate devices.

### 4. Run Backup
```bash
# Backup all devices
ansible-playbook forti_backup.yaml

# Backup specific group
ansible-playbook forti_backup.yaml --limit production_fortigates

# Backup single device
ansible-playbook forti_backup.yaml --limit fw-prod-01
```

## Directory Structure
```
fortigate-backup/
├── ansible.cfg
├── backup_fortigate.yaml
├── hosts.yaml
├── requirements.yaml
├── group_vars/
│   ├── all.yaml
│   └── fortigates/
│       ├── vars.yaml
│       └── vault.yaml
├── host_vars/
│   ├── fw-prod-01.yaml
│   ├── fw-prod-02.yaml
│   ├── fw-branch-01.yaml
│   ├── fw-branch-02.yaml
│   └── fw-dmz-01.yaml
└── backups/
└── backup.log
```

## Configuration

### Global Settings (group_vars/all.yaml)

- `backup_dir`: Backup storage location
- `retention_days`: Default retention period
- `backup_timestamp`: Timestamp format

### FortiGate Settings (group_vars/fortigates/vars.yaml)

- Connection parameters (HTTPS, SSL, timeouts)
- Authentication configuration
- FortiGate-specific settings

### Per-Device Settings (host_vars/)

- Device metadata (site, location, role)
- Custom retention periods
- Device-specific overrides

## Scheduling

### Using Cron
```bash
0 2 * * * cd /opt/fortigate-backup && source ~/.fortigate_credentials && ansible-playbook forti_backup.yaml
```

### Using Systemd

See deployment documentation for systemd timer setup.

## Security

- Use Ansible Vault for credential storage
- Set appropriate file permissions
- Secure backup directory
- Use dedicated API user with read-only access

## Troubleshooting

### Test Connectivity
```bash
ansible fortigates -m fortinet.fortios.fortios_monitor_fact -a "selector=system_status"
```

### Check Variables
```bash
ansible-inventory --host fw-prod-01
```

### Verbose Output
```bash
ansible-playbook forti_backup.yaml -vvv
```

## License

MIT

## Author

Ehsan Momeni Bashusqeh, Network Automation Engineer