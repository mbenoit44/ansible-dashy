# Ansible Playbook: Deployment of Dashy

## 📌 Overview
This Ansible playbook automates the deployment of [Dashy](https://github.com/Lissy93/dashy) using **Docker Compose** on a Synology NAS or any other compatible server. It ensures the application is properly configured and deployed with the latest updates.

## 📂 Directory Structure
```
└── mbenoit44-ansible-dashy/
    ├── README.md               # Documentation
    ├── ansible.cfg             # Ansible configuration
    ├── main.yml                # Main playbook
    ├── defaults/
    │   └── main.yml            # Default variables
    ├── files/
    │   ├── dashyconf.j2        # Dashy configuration template
    │   └── docker-compose.j2   # Docker Compose template
    ├── handlers/
    │   └── main.yml            # Handlers for Ansible tasks
    ├── meta/
    │   └── main.yml            # Metadata for the role
    ├── production/
    │   └── inventaire          # Inventory file for production
    ├── role/
    │   └── requirements.yml    # Required Ansible roles
    ├── tasks/
    │   ├── docker-compose.yml  # Docker Compose deployment tasks
    │   ├── docker.yml          # Docker installation tasks
    │   └── webhook.yml         # Webhook notification tasks
    ├── tests/
    │   ├── inventory           # Test inventory
    │   └── test.yml            # Test playbook
    └── vars/
        └── main.yml            # Custom variables
```

## 🛠️ Prerequisites
- A server running **Docker** and **Docker Compose**
- Ansible installed on your local machine
- SSH access to the target machine

## 🔧 Setup
### 1️⃣ Clone the Repository
```bash
git clone https://github.com/mbenoit44/mbenoit44-ansible-dashy.git
cd mbenoit44-ansible-dashy
```

### 2️⃣ Install Required Ansible Roles
```bash
ansible-galaxy install -r role/requirements.yml --force
```

### 3️⃣ Update the Inventory File
Modify `production/inventaire` to match your target server's IP or hostname:
```ini
[synology]
titans ansible_ssh_host=192.168.0.253
```

### 4️⃣ Define Custom Variables
Edit `vars/main.yml` if you need to customize the deployment:
```yaml
container_name: "Dashy"
image_name: "lissy93/dashy:latest"
host_port: 7444
container_port: 8080
docker_path: "/volume1/docker/dashy"
```

### 5️⃣ Run the Playbook
```bash
ansible-playbook -i production/inventaire main.yml
```

## 🚀 Playbook Details
### 📌 `main.yml`
This playbook:
1. Loads required roles (Docker Compose role)
2. Deploys the Dashy configuration and Docker Compose file
3. Starts the container
4. Sends a webhook notification after deployment

### 📌 `tasks/docker-compose.yml`
Handles:
- Copying `docker-compose.j2` template to the target machine
- Copying `dashyconf.j2` configuration
- Deploying the container

## 🧪 Testing
Run the test playbook to verify installation:
```bash
ansible-playbook -i tests/inventory tests/test.yml
```

## 🎯 Webhook Notifications
After deployment, a notification is sent to a Discord webhook:
```yaml
webhook_url: "https://discord.com/api/webhooks/..."
```
You can modify this in `vars/main.yml`.

## 📌 Notes
- Ensure the `docker-compose` binary is available on your target machine.
- Modify `ansible.cfg` to adjust user access settings if needed.

## 📜 License
MIT License. Contributions are welcome!