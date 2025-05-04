# Yolo Project: Stage 1 – Ansible Instrumentation

## ✅ Objective

This project demonstrates my ability to use Ansible to automate system configuration and containerized application deployment on a Vagrant-provisioned Ubuntu 20.04 server. It builds upon the containerization project completed in Week 2 by introducing modular automation, variables, roles, blocks, and tags for maintainability and scalability.

---

## 🗂️ Project Structure Overview

The Ansible project follows a standard best-practice structure:

```
project-root/
├── client/                    # Frontend application
│   ├── src/                   # Source code (React components, styles, etc.)
│   │   ├── components/
│   │   ├── pages/
│   │   ├── images/            # Images, icons, or other static assets
│   │   └── App.js
│   ├── public/                # Static files (HTML template, favicon, etc.)
│   ├── package.json           # Frontend dependencies
│   └── Dockerfile             # Dockerfile for frontend container
├── backend/                   # Backend application
│   ├── routes/                # Express routes for handling APIs
│   ├── models/                # MongoDB schemas and models
│   ├── server.js              # Backend entry point
│   ├── package.json           # Backend dependencies
│   └── Dockerfile             # Dockerfile for backend container
├── db/                        # MongoDB initialization scripts (optional)
│   └── init-db.js
├── docker-compose.yml         # Orchestration of all services
├── ansible/                   # Ansible automation scripts
│   ├── playbook.yml           # Defines provisioning tasks
│   ├── roles/                 # Modular automation roles
│   │   ├── system_config/     # Prepares environment and installs dependencies
│   │   ├── docker_setup/      # Installs and configures Docker
│   │   ├── frontend_setup/    # Deploys the frontend container
│   │   ├── backend_setup/     # Deploys the backend container
│   │   ├── mongo_setup/       # Deploys the MongoDB container
│   │   ├── legacy/            # Contains the deprecated app_deployment role for reference
│   │   │   ├── app_deployment/ # Previous deployment role (archived for historical reference)
├── vagrant/                   # Vagrant environment configuration
│   ├── Vagrantfile             # Defines VM setup and automation triggers
├── explanation.md             # Project explanation (documentation)
├── README.md                  # Project overview and setup instructions
└── .gitignore                 # Files to exclude from Git tracking
```

---

## 🧩 Core Components

### 🧭 Hosts File
I created a `hosts` file to serve as a static Ansible inventory. It contains the IP address of the Vagrant-managed virtual machine under the appropriate group:

```ini
[vagrant]
192.168.56.10 ansible_user=vagrant ansible_python_interpreter=/usr/bin/python3
```

### ⚙️ Ansible Configuration (`ansible.cfg`)
To simplify the CLI experience and avoid passing extra flags repeatedly, I created an `ansible.cfg` file with the following content:

```ini
[defaults]
inventory = hosts
host_key_checking = False
retry_files_enabled = False
```

This ensures Ansible uses the correct inventory file, skips host key checks, and disables retry file creation.

---

## 📦 Roles Implemented

Each major task has been broken into roles for separation of concerns and reusability:

### 1. `system_setup`
- Updates the apt index
- Installs required system packages
- Ensures the app directory exists
- Clones the project repository
- **Enhancements**: Used a `block` to group related tasks and a `system` tag for selective execution.

### 2. `docker_setup`
- Installs Docker and Docker Compose
- Adds Docker GPG key and repository
- Enables Docker service
- Adds `vagrant` user to Docker group
- **Enhancements**: Wrapped all tasks in a `block` and tagged them with `docker`. Key items like repo URL and package names were moved to `vars`.

### 3. `mongodb_setup`
- Starts MongoDB container using `docker_compose_v2`
- **Enhancements**: The `services` name and `project_src` were made configurable via variables. A `db` tag was added.

### 4. `backend`
- Starts the backend container using `docker_compose_v2`
- **Enhancements**: Used variables for service name and source directory. Tagged with `backend`.

### 5. `frontend`
- Starts the frontend container
- Adds an environment config line to support OpenSSL legacy provider
- **Enhancements**: Used variables and `frontend` tag. All frontend tasks wrapped in a `block`.

---

## 🧠 Concepts Demonstrated

### ✅ Variables
Used to store:
- Docker repo and GPG key
- Required system packages
- Docker/Compose package lists
- App repo URL and path
- Service names for containers

Variables are stored in each role’s `vars/main.yml` and shared ones under `group_vars/all/main.yml`.

### ✅ Roles
Tasks are modularized using Ansible roles for:
- System preparation
- Docker setup
- MongoDB containerization
- Backend and frontend deployment

This promotes maintainability and scalability.

### ✅ Blocks
Used to group logically related tasks (e.g., in `system_setup` and `docker_setup`) for clarity and better error handling.

### ✅ Tags
Tags such as `system`, `docker`, `frontend`, `backend`, and `db` allow selective task execution for faster development and testing:

```bash
ansible-playbook playbook.yml --tags docker
```

---

## 🖥️ Vagrant VM Setup

A `Vagrantfile` is used to spin up an Ubuntu 20.04 virtual machine. Jeff Geerling’s base box (`geerlingguy/ubuntu2004`) is used for consistency with class examples and ease of use (no key setup required).

### Key Commands

Start and provision the VM:
```bash
vagrant up
```

Access the VM:
```bash
vagrant ssh
```

Run the Ansible playbook:
```bash
ansible-playbook playbook.yml
```

Run specific tags (e.g., Docker-related tasks):
```bash
ansible-playbook playbook.yml --tags docker
```

---

## 🧪 Summary

This Stage 1 implementation shows effective use of:
- Ansible automation with modular roles
- Real-world infrastructure setup with Docker containers
- Best practices like blocks, tags, and variable abstraction
- Smooth provisioning with Vagrant and simplified Ansible configuration

This work lays the foundation for more advanced automation and orchestration in Stage 2.
