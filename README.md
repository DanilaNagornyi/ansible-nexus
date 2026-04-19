# Ansible Deployment: Nexus + Docker Stack

<p align="center">
  <img src="https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white" alt="Ansible" />
  <img src="https://img.shields.io/badge/Sonatype%20Nexus-3C3C3C?style=for-the-badge&logo=sonatype&logoColor=white" alt="Sonatype Nexus" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS" />
  <img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" alt="Terraform" />
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/DigitalOcean-0080FF?style=for-the-badge&logo=digitalocean&logoColor=white" alt="DigitalOcean" />
</p>

## Overview

This repository contains Ansible playbooks for two deployment flows:

- Deploying Sonatype Nexus on Ubuntu hosts (`nexus` group).
- Deploying Docker + Docker Compose application stack on AWS EC2 (`ec2_instances` group).

The latest update added Docker automation files (`deploy-docker.yaml`, `docker-compose-full.yaml`) and EC2 inventory support.

## Project Structure

```text
.
├── ansible.cfg               # Default Ansible settings (uses hosts inventory)
├── hosts                     # Inventory with nexus and ec2_instances groups
├── project-vars.yaml         # Shared variables (user, app version, Docker Hub creds)
├── deploy-nexus.yaml         # Nexus installation and startup playbook
├── deploy-docker.yaml        # Docker engine, Docker Compose, and container startup
├── docker-compose-full.yaml  # Multi-service compose file (java-app, mysql, phpmyadmin)
├── deploy-node.yaml          # Legacy Node.js deployment playbook
├── my-playbook.yaml          # Legacy test playbook
└── nexus.sh                  # Manual Nexus setup helper script
```

## Prerequisites

- Ansible installed on your local machine.
- SSH access to target servers with the key configured in `hosts`.
- For Docker deployment: AWS EC2 host using a yum-based distro (for example Amazon Linux).
- Docker collection for Ansible modules:

```bash
ansible-galaxy collection install community.docker
```

## Configuration

### 1) Inventory (`hosts`)

Update IPs and connection settings for both groups:

```ini
[nexus]
<NEXUS_HOST_IP>

[ec2_instances]
<EC2_HOST_IP>
```

Current inventory uses:

- `nexus` as `root` with Python 3.12 interpreter.
- `ec2_instances` as `ec2-user`.

### 2) Project variables (`project-vars.yaml`)

Set your values before running playbooks:

```yaml
location: /path/to/nodejs-app
version: 1.0.0
linux_name: appuser
user_home_dir: /home/{{linux_name}}
user_groups: adm,docker

dockerhub_username: your_dockerhub_username
dockerhub_password: your_dockerhub_password
```

## Run Playbooks

`ansible.cfg` already points to `hosts`, so `-i hosts` is optional.

### Deploy Nexus

```bash
ansible-playbook deploy-nexus.yaml
```

### Deploy Docker stack on EC2

```bash
ansible-playbook deploy-docker.yaml
```

### Legacy Node.js deployment

```bash
ansible-playbook deploy-node.yaml
```

## Notes

- `deploy-node.yaml` and `my-playbook.yaml` use host group `webserver`, which is not present in the current `hosts` file.
- `deploy-docker.yaml` copies `docker-compose-full.yaml` to `/home/{{linux_name}}/docker-compose.yml` and starts services from that directory.

## License

This project is open-source. Feel free to use and modify it for your infrastructure needs.
