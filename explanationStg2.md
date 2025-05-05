# Stage 2: Infrastructure Provisioning and Configuration with Ansible

## 📘 Overview
NB For stage 1 explanation file checkout to master branch

In Stage 2, we replicate the architecture from Stage 1, but with a fresh environment. We use **Ansible alone** to handle everything — from virtual machine provisioning (via Vagrant) to system setup and containerized application deployment.

Although Terraform was optional, this implementation **excludes Terraform** to demonstrate a streamlined approach using Ansible only.

---

## 🗂 Directory Structure

```
stage-1-Ansible-root/
├── Stage_two/
│   ├── ansible/                 # Ansible configuration files
│   │   ├── playbook.yml         # Main Ansible playbook
│   │   └── roles/               # Ansible roles
│   ├── vagrant/                 # Vagrant VM definition
│   │   └── Vagrantfile
│   └── hosts                    # Ansible inventory file
```

---

## 🏗️ 1. Resource Provisioning (Using Vagrant)

Provisioning is handled by Vagrant, which sets up an Ubuntu-based VM with required specifications.

- **Provisioning Tool**: Vagrant
- **Configuration File**: `vagrant/Vagrantfile`
- **Execution Command**:
  ```bash
  
  vagrant up
  ```

This sets up the virtual machine that Ansible will target via SSH.

---

## ⚙️ 2. Server Configuration & Application Deployment (Using Ansible)

Ansible executes a series of roles to install dependencies, configure services, and deploy the full stack. The roles are executed **sequentially** in the following order to ensure logical dependencies are satisfied.

### 🔄 Role Execution Order and Purpose

#### 1. `system_config`
- Updates and upgrades system packages.
- Installs basic dependencies like `curl`, `ca-certificates`, etc.
- Prepares the system for Docker installation.
- **Modules used**: `apt`, `package`, `command`, `systemd`

#### 2. `docker_setup`
- Installs Docker CE and Docker Compose plugin.
- Adds the `vagrant` user to the Docker group.
- Verifies Docker is running properly.
- **Modules used**: `apt`, `get_url`, `unarchive`, `shell`, `service`

#### 3. `mongo_setup`
- Deploys MongoDB as a Docker container using Docker Compose.
- Creates data volumes if needed.
- **Module used**: `community.docker.docker_compose_v2`
- **Key vars**:
  - `mongo_project_src`
  - `mongo_service_name`

#### 4. `backend_setup`
- Pulls and deploys the Node.js backend using Docker Compose.
- Connects to the running MongoDB container.
- **Module used**: `community.docker.docker_compose_v2`
- **Key vars**:
  - `backend_project_src`
  - `backend_service_name`

#### 5. `frontend_setup`
- Deploys the React frontend using Docker Compose.
- Makes the application accessible via the host port.
- **Module used**: `community.docker.docker_compose_v2`
- **Key vars**:
  - `frontend_project_src`
  - `frontend_service_name`

---

## ✅ Execution Flow

1. **Provision VM**:
   ```bash
   cd Stage_two
   vagrant up
   ```

2. **Run Ansible Playbook**:
   ```bash
   cd ../ansible
   ansible-playbook -i ../hosts playbook.yml
   ```

---

## 🛠️ Ansible Module Note

The following Ansible collection must be installed inside the VM to support Docker Compose tasks:

```bash
ansible-galaxy collection install community.docker
```

Run this after SSHing into the Vagrant VM:
```bash
vagrant ssh
```

---

## 🧪 Testing and Validation

To confirm successful provisioning and deployment:

- Run `docker ps` inside the VM:
  ```bash
  vagrant ssh
  docker ps
  ```

- All services (MongoDB, Backend, Frontend) should appear as running containers.
- Visit the frontend app via the exposed port in the browser:
  ```
  http://localhost:<frontend_port>
  ```

---

## 📌 Summary

| Task                     | Tool Used | Description                                 |
|--------------------------|-----------|---------------------------------------------|
| VM Provisioning          | Vagrant   | Creates the Ubuntu VM environment           |
| Server Configuration     | Ansible   | Installs Docker and system dependencies     |
| Application Deployment   | Ansible   | Starts MongoDB, Backend, and Frontend stack |

---
