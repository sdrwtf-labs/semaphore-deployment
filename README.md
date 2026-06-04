# Ansible Semaphore Infrastructure

This repository contains the deployment configuration for Ansible Semaphore and its associated PostgreSQL database. It assumes a pre-configured Docker host is already available.

## Prerequisites
- A working Docker host (refer to the `infra-notes` repository for base host provisioning).
- Git installed on the host.

## Deployment

1. Prepare the deployment directory and clone this repository:
```bash
sudo mkdir -p /opt/semaphore
sudo chown $USER:$USER /opt/semaphore
cd /opt/semaphore

git clone git@github.com:sdrwtf-labs/semaphore-deployment.git .
```

2. Generate 

3. Create a .env file in this directory with the following variables:

```text
## Host configuration
HOST_PORT=80
LOCAL_DNS=replace_with_local_dns

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

## Semaphore Encryption Key
SEMAPHORE_ACCESS_KEY_ENCRYPTION=replace_with_generated_base64_key

## SMTP Configuration
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_SENDER=notifications@example.com
# Comment out if user/password not needed
SMTP_USER=your_username
SMTP_PASS=your_smtp_password
```

3. Start the services:

```bash
docker compose up -d
```

## Repository Structure

- `compose.yaml`: Docker service definitions.
- `.gitignore`: Ensures the `.env` file (containing secrets) is never pushed to GitHub.
