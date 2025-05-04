# Yolo Project: Stage 1 – Ansible Instrumentation

## ✅ Objective

This project demonstrates my ability to use Ansible to automate system configuration and containerized application deployment on a Vagrant-provisioned Ubuntu 20.04 server. It builds upon the containerization project completed in Week 2 by introducing modular automation, variables, roles, blocks, and tags for maintainability and scalability.

---
# Explanation of Playbook Execution and Roles

## 🎯 Objective

This explanation provides an overview of the structure, reasoning, and execution order of tasks within the Ansible playbook. Each role and task is explained in terms of its function, position in the playbook, and the Ansible modules used for system configuration.

---

## 🧩 Playbook Execution Flow

The playbook is executed sequentially, with each task and role positioned to ensure that the environment is properly set up before the next phase begins. Below is an explanation of the roles and their order of execution.

### 1. **System Configuration (`system_config`)**

**Position**: This role runs first in the playbook.

**Function**:
- Prepares the environment by ensuring the system has the necessary dependencies and directories set up.
- Clones the project repository from GitHub to the `/home/vagrant/app` directory.

**Modules Used**:
- **`apt`**: Updates the package index and installs necessary packages (`apt-transport-https`, `ca-certificates`, etc.) to allow the system to download Docker and other software.
- **`file`**: Ensures the `/home/vagrant/app` directory exists for the project repository.
- **`git`**: Clones the project repository into the `/home/vagrant/app` directory.

### 2. **Docker Setup (`docker_setup`)**

**Position**: This role runs after the system configuration role to ensure that Docker and Docker Compose are installed and ready for use.

**Function**:
- Installs Docker, Docker Compose, and necessary dependencies.
- Configures the Docker service and ensures it starts automatically.
- Adds the `vagrant` user to the Docker group to allow running Docker commands without `sudo`.

**Modules Used**:
- **`apt_key`**: Adds Docker’s official GPG key for verifying Docker packages.
- **`apt_repository`**: Adds Docker’s repository to the system's package manager to enable Docker installation.
- **`apt`**: Installs Docker, Docker Compose, and required dependencies.
- **`systemd`**: Ensures the Docker service is enabled and started.
- **`user`**: Adds the `vagrant` user to the Docker group for permissions.

### 3. **MongoDB Setup (`mongo_setup`)**

**Position**: This role comes after Docker is installed. It ensures that MongoDB is set up in a container and ready for the backend service.

**Function**:
- Uses Docker Compose to start the MongoDB container, ensuring the database is available for the backend service.

**Modules Used**:
- **`docker_compose_v2`**: Deploys the MongoDB service, ensuring it starts with the proper configurations.

### 4. **Backend Setup (`backend_setup`)**

**Position**: This role is executed after MongoDB is set up to ensure that the backend service is deployed after the database is ready.

**Function**:
- Uses Docker Compose to start the backend container, which connects to the frontend and provides API services.
- Ensures that the backend service is properly containerized and connected to the MongoDB database.

**Modules Used**:
- **`docker_compose_v2`**: Deploys the backend service, ensuring it runs with the necessary configurations.

### 5. **Frontend Setup (`frontend_setup`)**

**Position**: This role runs last to ensure that the frontend application is deployed after the backend and database are set up and available.

**Function**:
- Uses Docker Compose to start the frontend container, which interacts with the backend and provides the user interface.

**Modules Used**:
- **`docker_compose_v2`**: Deploys the frontend service, ensuring it starts with the correct configurations (e.g., always recreate the service, remove orphan containers).

---

## 🧑‍💻 Reasoning for Task and Role Order

- **System Configuration**: The environment needs to be ready before anything else, which is why the system setup role is executed first. This includes ensuring necessary software dependencies are available (like Git, curl, etc.) and the project is cloned.

- **Docker Setup**: Docker is the foundation of containerization, so it needs to be installed and configured early. Once Docker is available, the system can begin containerizing and managing the services.

- **MongoDB Setup**: MongoDB is set up before the backend to ensure the database is available when the backend service starts. This ensures that the backend can immediately connect to the database when it starts.

- **Backend Setup**: The backend is deployed after the MongoDB container is ready, ensuring the service has a database to connect to and can provide API functionality.

- **Frontend Setup**: Finally, the frontend is deployed after the backend and database are available, ensuring the application can interact with the backend APIs and serve the user interface.

---

## 🧰 Modules Used

### 1. **`apt`**:
   - **Purpose**: Installs packages on the target system (e.g., Docker, Git).
   - **Used in**: `system_config`, `docker_setup`.

### 2. **`git`**:
   - **Purpose**: Clones repositories from GitHub or other version control systems.
   - **Used in**: `system_config`.

### 3. **`docker_compose_v2`**:
   - **Purpose**: Manages Docker Compose services (e.g., frontend, backend, MongoDB).
   - **Used in**: `frontend_setup`, `backend_setup`, `mongo_setup`.

### 4. **`apt_key`**:
   - **Purpose**: Adds a GPG key for verifying the authenticity of Docker packages.
   - **Used in**: `docker_setup`.

### 5. **`apt_repository`**:
   - **Purpose**: Adds a repository to the APT package manager.
   - **Used in**: `docker_setup`.

### 6. **`systemd`**:
   - **Purpose**: Manages system services (e.g., enabling Docker to start on boot).
   - **Used in**: `docker_setup`.

### 7. **`user`**:
   - **Purpose**: Modifies user properties, such as adding users to groups.
   - **Used in**: `docker_setup`.

### 8. **`file`**:
   - **Purpose**: Ensures that a file or directory exists with the correct properties.
   - **Used in**: `system_config`.

---

## 📈 Conclusion

The playbook is structured to set up the environment in a logical sequence: 
1. Prepare the system.
2. Install and configure Docker.
3. Deploy MongoDB, followed by the backend service, and finally the frontend.

Each role focuses on a specific part of the setup, and the playbook’s modular structure makes it easier to maintain and scale the project in the future.


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
