# Ansible Infrastructure Setup

## Overview

This project automates the deployment and configuration of a complete DevOps infrastructure using Ansible. It sets up multiple machines with containerized applications including Jenkins, SonarQube, Nexus, GitLab, and Portainer.

## Project Structure

```
ansible-infra/
├── inventory.yml              # Hosts configuration
├── ansible.cfg               # Ansible configuration
├── playbooks/
│   └── setup.yml            # Main playbook
├── roles/
│   ├── common/              # Base packages and tools
│   ├── docker/              # Docker installation
│   ├── jenkins/             # Jenkins CI/CD
│   ├── sonarqube/           # Code quality analysis
│   ├── nexus/               # Artifact repository
│   ├── glab/                # GitLab server
│   ├── nginx_certbot/       # Nginx reverse proxy with SSL
│   ├── portainer/           # Docker management UI
│   ├── ohmyzsh/             # Shell customization
│   ├── helm_repo/           # Helm repository
│   └── repository/          # Package repository
├── group_vars/
│   └── all.yml             # Global variables
└── README.md
```

## Machines Configuration

- **machine01** (192.168.64.9): Primary server with Jenkins, SonarQube, Nexus, GitLab, and Nginx
- **machine02** (192.168.64.6): Secondary server with Portainer and utilities
- **machine03** (192.168.64.7): Tertiary server with Portainer, Helm, and Repository tools

## Prerequisites

- Ansible 2.9+
- SSH access to all machines
- Ubuntu 22.04 LTS on all machines
- Minimum 100GB disk space on machine01
- Domain names configured (optional for HTTPS)

## Installation

1. Clone the repository:
```bash
cd /Users/sivthingthing/Desktop/Task01/ansible-infra
```

2. Update inventory with your machine IPs:
```bash
vim inventory.yml
```

3. Configure domain names (optional):
```bash
vim group_vars/all.yml
```

4. Run the playbook:
```bash
ansible-playbook playbooks/setup.yml
```

## Services and Access

### Jenkins
- **Port**: 8080
- **URL**: http://jenkin.sivthingthing.xyz or http://localhost:8080
- **Purpose**: CI/CD automation

### SonarQube
- **Port**: 9001
- **URL**: http://sonarqube.sivthingthing.xyz or http://localhost:9001
- **Purpose**: Code quality and security analysis

### Nexus
- **Port**: 8081
- **URL**: http://nexus.sivthingthing.xyz or http://localhost:8081
- **Purpose**: Artifact and package repository management

### Portainer
- **Port**: 9000
- **URL**: http://portainer.sivthingthing.xyz:9000 or http://localhost:9000
- **Purpose**: Docker container management UI

### GitLab
- **Port**: 80/443
- **Purpose**: Git repository and CI/CD platform

## Configuration Files

- `inventory.yml`: Define hosts and IP addresses
- `ansible.cfg`: Ansible settings and role paths
- `group_vars/all.yml`: Global variables (domains, users)
- `roles/*/tasks/main.yml`: Role-specific tasks
- `roles/*/templates/`: Configuration templates (Nginx, etc.)

## Running Specific Roles

Run only specific roles on targeted machines:

```bash
ansible-playbook playbooks/setup.yml -l machine01 -v
ansible-playbook playbooks/setup.yml --tags "jenkins"
```

## Common Commands

Check syntax:
```bash
ansible-playbook playbooks/setup.yml --syntax-check
```

Dry run (no changes):
```bash
ansible-playbook playbooks/setup.yml --check
```

Verbose output:
```bash
ansible-playbook playbooks/setup.yml -vvv
```

## Troubleshooting

### APT Lock Issues
If apt is locked, the playbook automatically waits or kills the process. If manual intervention is needed:
```bash
ssh ubuntu@<machine-ip>
sudo pkill -9 apt-get
```

### Disk Space
If running out of space:
```bash
docker system prune -a --volumes
```

### Nginx Configuration Errors
Test nginx configuration:
```bash
sudo nginx -t
sudo systemctl reload nginx
```

### Docker Container Issues
Check container logs:
```bash
docker logs <container-name>
docker ps -a
```

## SSL/HTTPS Setup

The `nginx_certbot` role automatically configures SSL certificates. Ensure your domains are properly configured in DNS before running the playbook.

To manually generate certificates:
```bash
sudo certbot certonly --standalone -d jenkin.sivthingthing.xyz
```

## Notes

- All services run in Docker containers for easy management and scaling
- Nginx acts as a reverse proxy for all services
- Database services (PostgreSQL for SonarQube) run in separate containers
- User credentials and API keys should be changed after first login
- Regular backups of `/opt/*` directories are recommended

## Support

For issues or questions, check service logs:
```bash
docker logs <service-name>
journalctl -xeu <service-name>.service
```

Refer to official documentation:
- Jenkins: https://www.jenkins.io/doc/
- SonarQube: https://docs.sonarqube.org/
- Nexus: https://help.sonatype.com/repomanager3
- GitLab: https://docs.gitlab.com/
- Portainer: https://docs.portainer.io/


# By Siv Thingthing