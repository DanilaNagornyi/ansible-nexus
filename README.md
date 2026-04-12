# Ansible Deployment: Nexus & Node.js on DigitalOcean

<p align="center">
  <img src="https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white" alt="Ansible" />
  <img src="https://img.shields.io/badge/Sonatype%20Nexus-3C3C3C?style=for-the-badge&logo=sonatype&logoColor=white" alt="Sonatype Nexus" />
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/DigitalOcean-0080FF?style=for-the-badge&logo=digitalocean&logoColor=white" alt="DigitalOcean" />
</p>

## 📖 Overview

This project provides a set of Ansible playbooks designed to automate the deployment and configuration of **Sonatype Nexus Repository Manager** and a **Node.js application** on **DigitalOcean** droplets (Ubuntu-based).

The automation handles everything from environment preparation (Java, Node.js installation) to application lifecycle management, including user creation and service startup.

---

## 🏗 Project Structure

```text
.
├── ansible.cfg          # Ansible configuration file
├── hosts                # Inventory file (IPs and connection vars)
├── deploy-nexus.yaml    # Playbook for Sonatype Nexus deployment
├── deploy-node.yaml     # Playbook for Node.js application deployment
├── project-vars.yaml    # Shared variables (versions, paths, users)
├── my-playbook.yaml     # Main entry point or testing playbook
└── nexus.sh             # Helper shell script for Nexus operations
```

---

## 🚀 Getting Started

### Prerequisites

*   **Ansible** installed on your local machine.
*   **DigitalOcean Droplets** (Ubuntu) ready with SSH access.
*   SSH keys configured for the `root` user (specified in `hosts`).

### Configuration

1.  **Update Inventory:**
    Modify the `hosts` file with your Droplet's IP address:
    ```ini
    [nexus]
    <YOUR_DROPLET_IP>
    ```

2.  **Adjust Variables:**
    Update `project-vars.yaml` with your specific configuration:
    ```yaml
    location: /path/to/your/local/app
    version: 1.0.0
    linux_name: your_app_user
    ```

### Running Playbooks

#### Deploy Nexus Repository Manager
This playbook installs Java, downloads Nexus, sets up the `nexus` user, and starts the service.
```bash
ansible-playbook -i hosts deploy-nexus.yaml
```

#### Deploy Node.js Application
This playbook installs Node.js/NPM, creates an application user, extracts the app, and starts it.
```bash
ansible-playbook -i hosts deploy-node.yaml
```

---

## 🛠 Features

*   **Automated Provisioning:** Zero manual configuration on the target servers.
*   **Idempotent Execution:** Safe to run multiple times.
*   **Security Best Practices:** Creates dedicated non-root users (`nexus`, `appuser`) to run applications.
*   **Verification Steps:** Automated health checks using `netstat` and `ps` to ensure services are running correctly.

---

## ⚖️ License
This project is open-source. Feel free to use and modify it for your infrastructure needs.
