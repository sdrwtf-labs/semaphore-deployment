# Ansible Semaphore Infrastructure

This repository hosts the Ansible Semaphore UI and associated services for automated system administration, running on a dedicated Debian VM.

## Prerequisites & Installation
This setup assumes a fresh Debian 13 (Trixie) VM. Run the following commands to prepare the environment:

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker Engine and Compose
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL [https://download.docker.com/linux/debian/gpg](https://download.docker.com/linux/debian/gpg) | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] [https://download.docker.com/linux/debian](https://download.docker.com/linux/debian) $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Optional: Install QEMU Guest Agent for Proxmox integration
sudo apt install -y qemu-guest-agent
sudo systemctl enable --now qemu-guest-agent
```

## Setup
1. Create a `.env` file in this directory with the following variables:
   ```text
   ## Host configuration
   HOST_PORT=80

   ## Database credentials
   DB_USER=semaphore
   DB_NAME=semaphore
   DB_PORT=5432
   DB_PASS=replace_with_a_secure_password

   ## Semaphore Admin credentials
   ADMIN_PASS=replace_with_a_secure_admin_password
   ADMIN_NAME=Admin
   ADMIN_USER=admin
   ADMIN_EMAIL=admin@your-domain.local

   ## SMTP Configuration
   SMTP_HOST=smtp.example.com
   SMTP_PORT=587
   SMTP_SENDER=notifications@example.com
   # Comment out if user/password not needed
   SMTP_USER=your_username
   SMTP_PASS=your_smtp_password
   ```
2. Start the services:
   `docker compose up -d`

## Repository Structure
- `compose.yaml`: Docker service definitions.
- `.gitignore`: Ensures the `.env` file (containing secrets) is never pushed to GitHub.
