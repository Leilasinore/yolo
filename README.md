# Yolo Project: Stage 1 – Ansible Instrumentation

## 📝 Project Overview

This project demonstrates the automation of system configuration, Docker installation, and containerization of a web application using Ansible on a Vagrant-managed virtual machine. The task leverages the tools covered in the course, combining concepts like variables, roles, blocks, tags, and the provisioning of a Vagrant virtual machine (VM) for automated system setup.

NB:For a detailed explanation of implementation details checkout the explanation.md  in the root of this repo

## 🛠️ Technologies & Tools Required


- **Vagrant**: Provisioning and managing the virtual machine.
- **Docker**: Containerization of the frontend, backend, and MongoDB.
- **Git**: Used for cloning the application repository.
- - **Ansible**: Used for running playbooks.

---

## ⚙️ Setup Instructions

### 1. **Clone the Repository**

```bash
git clone https://github.com/Leilasinore/yolo.git
cd yolo
```

### 2. **Vagrant Setup**

Ensure you have Vagrant installed. If not, [install Vagrant](https://www.vagrantup.com/downloads).

Start the Vagrant virtual machine and provision it:

```bash
vagrant up
```

This will:
- Download the Ubuntu 20.04 box (if not already downloaded).
- Provision the VM with the necessary configurations defined in the playbooks.
Open your browser and visit http://localhost:3000 to access the application
### 3. **Ansible Playbook Execution**

Ensure Ansible is installed. If not, [install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html).

Run the Ansible playbook to set up the environment and install required packages and containers:

```bash
ansible-playbook ansible/playbook.yml
```

To run specific tags (e.g., `docker` tag):

```bash
ansible-playbook ansible/playbook.yml --tags docker
```

---

## 🧩 Project Structure

The project is organized as follows:

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

## 🛠️ Key Features & Tasks

- **Provisioning VM**: The `Vagrantfile` provisions an Ubuntu 20.04 VM for containerization.
- **System Setup**: Installs necessary system packages and clones the project repository.
- **Docker Setup**: Automates Docker installation, Docker Compose setup, and adds the user to the Docker group.
- **MongoDB Setup**: Deploys MongoDB using Docker Compose.
- **Frontend & Backend Setup**: Deploys the frontend and backend applications using Docker Compose.

---

## 🧠 Concepts Demonstrated

- **Ansible Roles**: Modularized tasks for better structure and reusability.
- **Variables**: Abstracted configuration values for easy customization.
- **Blocks**: Grouped related tasks for better readability and error handling.
- **Tags**: Used for selective execution of tasks (e.g., `docker`, `frontend`, `backend`, etc.).

---

## 🧪 Running the Project

### 1. **Start the VM** using Vagrant:

```bash
vagrant up
```

2. **Execute the Ansible playbook** to apply all configurations:

```bash
ansible-playbook ansible/playbook.yml
```

   Or run specific parts of the playbook using tags (e.g., `docker` tag):

```bash
ansible-playbook ansible/playbook.yml --tags docker
```

3. **Access the VM** for debugging or manual configuration:

```bash
vagrant ssh
```

---

## 🏆 Outcome

This project sets up an environment with automated Docker and application containerization, using Ansible for configuration management. The VM provisioning, Docker setup, and container deployment are fully automated, demonstrating key concepts in DevOps practices.

---

## 🔄 Future Improvements

- Integrate more advanced container orchestration techniques (e.g., Kubernetes).
- Implement CI/CD pipeline for the project.
- Extend the playbook to include more complex system monitoring and alerting configurations.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
