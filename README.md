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

This repository contains Ansible playbooks for several infrastructure/deployment scenarios:

- Sonatype Nexus installation on Ubuntu hosts (`nexus` group).
- Docker stack deployment on AWS EC2 (`ec2_instances` group).
- A roles-based Docker deployment flow (`create_user`, `start_containers`).
- Optional dynamic EC2 inventory via `aws_ec2` plugin.
- Optional Kubernetes deployment example.

## Project Structure

```text
.
├── ansible.cfg
├── hosts
├── inventory_aws_ec2.yaml
├── project-vars.yaml
├── deploy-nexus.yaml
├── deploy-docker.yaml
├── deploy-docker-new-user.yaml
├── deploy-docker-with-roles.yaml
├── docker-compose-full.yaml
├── deploy-node.yaml
├── deploy-to-k8s.yaml
├── my-playbook.yaml
├── nexus.sh
└── roles/
    ├── create_user/
    │   ├── defaults/main.yaml
    │   └── tasks/main.yaml
    └── start_containers/
        ├── files/docker-compose.yaml
        ├── tasks/main.yaml
        └── vars/main.yaml
```

## Roles-Based Flow

`deploy-docker-with-roles.yaml` splits deployment into reusable blocks:

- Install Docker on target hosts.
- Create Linux user via role `create_user`.
- Install Docker Compose for that user.
- Start application stack via role `start_containers`.

This is the most extensible way in this repository to scale playbooks for multiple environments.

## Prerequisites

- Ansible installed locally.
- SSH access to target servers.
- For Docker deployment: a yum-based Linux host (for example Amazon Linux).
- Required Ansible collections:

```bash
ansible-galaxy collection install community.docker
ansible-galaxy collection install amazon.aws
ansible-galaxy collection install kubernetes.core
```

For dynamic AWS inventory (`inventory_aws_ec2.yaml`), configure AWS credentials on your machine.

## Configuration

### 1) Static Inventory (`hosts`)

Example groups used in this project:

```ini
[nexus]
<NEXUS_HOST_IP>

[ec2_instances]
<EC2_HOST_IP>
```

### 2) Project Variables (`project-vars.yaml`)

Set your values before running Docker-related playbooks:

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

### Deploy Docker (single playbook flow)

```bash
ansible-playbook deploy-docker.yaml
```

### Deploy Docker (roles-based flow)

```bash
ansible-playbook deploy-docker-with-roles.yaml
```

### Deploy with dynamic AWS inventory

```bash
ansible-playbook -i inventory_aws_ec2.yaml deploy-docker.yaml
```

### Optional Kubernetes demo

```bash
ansible-playbook deploy-to-k8s.yaml
```

## Notes

- `deploy-node.yaml` and `my-playbook.yaml` are legacy examples.
- Docker deployment uses Docker Hub credentials from variables.
- The roles folder is ready for further extension (for example, adding `install_docker` as a dedicated role).

## License

This project is open-source. Feel free to use and modify it for your infrastructure needs.
