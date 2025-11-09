# Cloud-Native Web Application Deployment
# Developed by: Hussain Amin Manj

This project implements a complete cloud infrastructure and deployment pipeline for a web application using Azure, Terraform, Docker, and Ansible.

## Project Structure

```
.
├── ansible/
│   ├── deploy-docker.yaml    # VM configuration and container deployment
│   └── inventory.ini         # Target hosts inventory
├── app/
│   ├── Dockerfile           # nginx container configuration
│   └── index.html          # Web application content
├── terraform/
│   ├── main.tf             # Azure infrastructure definition
│   ├── variables.tf        # Variable declarations
│   └── output.tf           # Output configurations
└── .github/
    └── workflows/
        └── deploy.yml      # CI/CD pipeline
```

## Prerequisites

- Azure subscription
- Terraform installed locally
- Docker Hub account
- GitHub account (for Actions)
- SSH key pair for VM access

## Setup Steps

### 1. Infrastructure Deployment (Terraform)

```bash
cd terraform
# Create terraform.tfvars with your values
# Required: subscription_id, ssh_public_key_path

terraform init
terraform plan
terraform apply
```

### 2. Configure GitHub Secrets

Add these secrets to your GitHub repository:
- `DOCKER_USERNAME`: Your Docker Hub username
- `DOCKER_PASSWORD`: Your Docker Hub password
- `VM_HOST`: VM's public IP (from terraform output)
- `VM_SSH_USER`: VM username (default: azureuser)
- `VM_SSH_KEY`: Your private SSH key

### 3. Application Deployment

The GitHub Actions workflow automatically:
1. Builds the Docker image
2. Pushes to Docker Hub
3. Deploys to VM using Ansible

Manual deployment possible in WSL:
```bash
cd ansible
ansible-playbook -i inventory.ini deploy-docker.yaml
```

## Components

### Infrastructure (Terraform)
- Virtual Network with subnet
- Linux VM (Ubuntu 20.04)
- Network Security Group (ports 22, 80)
- Public IP

### Configuration (Ansible)
- Installs Docker
- Manages container lifecycle
- Handles deployment updates

### Application
- nginx-based container
- Static web content
- Automated builds

### CI/CD (GitHub Actions)
- Triggered on push to "main" branch
- Automated deployment pipeline
- Secure secrets handling

## Security Notes

- Keep terraform.tfvars out of version control
- Use GitHub Secrets for sensitive data
- SSH key-based authentication
- Restricted network access

## Maintenance

### Updating the Application
1. Modify files in `app/`
2. Commit and push to main
3. GitHub Actions handles deployment

### Infrastructure Changes
1. Modify Terraform files
2. Run:
```bash
terraform plan
terraform apply
```

### Adding New VM Configuration
1. Update `ansible/deploy-docker.yaml`
2. Commit and push, or run manually

## Troubleshooting

### Common Issues
1. Container not updating?
   - Check GitHub Actions logs
   - Verify Docker Hub push succeeded
   - Check Ansible deployment logs

2. Can't access website?
   - Verify NSG rules
   - Check container status on VM
   - Verify Docker service is running

### Useful Commands

```bash
# Check container status
docker ps
docker logs webapp

# Restart container
docker restart webapp

# Check nginx status
sudo systemctl status nginx
```
