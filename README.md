# N8N Server Setup with Ansible

Simple Ansible playbook to set up N8N on a server with Docker and Caddy for automatic HTTPS.

## Prerequisites

1. Ansible installed on your local machine
2. SSH access to your target server
3. Python installed on the target server

## Installing Ansible on Mac

1. Install Homebrew if not already installed:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Install Ansible:

```bash
brew install ansible
```

3. Verify installation:

```bash
ansible --version
```

## Domain Configuration

1. Point your domain to your server's IP address by adding these DNS records:

   - Type: A
   - Name: n8n (or your chosen subdomain)
   - Value: YOUR_SERVER_IP
   - TTL: 3600 (or as preferred)

2. Edit `group_vars/all.yml` and update:
   ```yaml
   domain_name: "yourdomain.com" # Your actual domain
   subdomain: "n8n" # Your chosen subdomain
   ```

## Quick Start

1. Edit `inventory.yml` and replace the IP address:

```yaml
ansible_host: YOUR_SERVER_IP
```

2. Edit `group_vars/all.yml` and update:

   - Your SSH public key
   - Domain settings (as above)
   - Passwords (docker user)

3. Run the playbook:

```bash
ansible-playbook -i inventory.yml playbook.yml
```

## Adding Custom NPM Packages

To use external NPM packages in your n8n functions:

1. Add the package to `templates/Dockerfile.j2`:

```dockerfile
# Install custom npm packages
RUN npm install -g your-package-name
# Add any other npm packages you need here
```

2. Allow the package in `templates/docker-compose.yml.j2`:

```yaml
environment:
  - NODE_FUNCTION_ALLOW_EXTERNAL=your-package-name # Allow external module
```

## After Installation

- N8N will be available at: `https://n8n.yourdomain.com`
- Caddy will automatically handle SSL certificates
- First time setup will ask you to create an admin account

## Security Notes

- Root login via SSH will be disabled
- Password authentication will be disabled
- Only SSH key authentication will work
- Docker user will have sudo access
- UFW firewall is configured to allow only ports 22, 80, 443
