# 🚀 Stage 2: Ansible-Only Infrastructure Automation

Welcome to **Stage 2** of the DevOps project! In this stage, we rebuild the application environment from **Stage 1**, to set up a new environment. We'll rely entirely on **Ansible and Vagrant**, skipping Terraform to demonstrate full automation with Ansible alone.

---

## 📁 Project Structure

```
Stage_two/
├── ansible/
│   ├── playbook.yml
│   └── roles/
│       ├── system_config/
│       ├── docker_setup/
│       ├── mongo_setup/
│       ├── backend_setup/
│       └── frontend_setup/
├── vagrant/
│   └── Vagrantfile
└── hosts
```

---

## ⚙️ Tools Used

- **Vagrant** — for VM provisioning
- **Ansible** — for configuration management and app deployment
- **Docker & Docker Compose** — for containerized services

---

## 🧰 Setup Instructions

### 1. Clone and Checkout Branch

```bash
git checkout -b Stage_two
```

Create a new directory `Stage_two` inside `stage-1-Ansible-root`.

### 2. Provision the Virtual Machine

```bash
cd Stage_two
vagrant up
```

This sets up a new Ubuntu-based virtual machine for deployment.

### 3. Install Required Ansible Collections (Inside VM)

SSH into the VM and install Docker modules:

```bash
vagrant ssh
ansible-galaxy collection install community.docker
```

### 4. Run the Ansible Playbook

Back on your host machine:

```bash
cd Stage_two
ansible-playbook -i ../hosts playbook.yml
```

This will configure the server and deploy MongoDB, the backend, and the frontend in sequence using Ansible roles.

---

## 🏗️ Roles Overview

| Role Name        | Purpose                                      |
|------------------|----------------------------------------------|
| system_config     | Updates system and installs dependencies     |
| docker_setup      | Installs Docker and Docker Compose plugin    |
| mongo_setup       | Deploys MongoDB container                    |
| backend_setup     | Deploys the Node.js backend container        |
| frontend_setup    | Deploys the React frontend container         |

---

## 🧪 Testing the Setup

1. SSH into the VM:
   ```bash
   vagrant ssh
   ```

2. Check if containers are running:
   ```bash
   docker ps
   ```

3. Visit the frontend in your browser:
   ```
   http://localhost:3000
   ```

---

## ✅ Success Criteria

- MongoDB container is running
- Backend container is connected to MongoDB
- Frontend is accessible and consuming the backend API
- All services defined in `docker-compose.yml` are running

---

## 📚 References

- [Ansible Docs](https://docs.ansible.com/)
- [Docker Compose Module](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_compose_v2_module.html)
- [Vagrant Docs](https://developer.hashicorp.com/vagrant/docs)

---

> 🔁 This stage reinforces end-to-end automation using just **Ansible + Vagrant** with a production-like containerized app stack.

