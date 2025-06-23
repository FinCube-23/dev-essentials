# CI/CD Pipeline Setup Guide

This README provides instructions for setting up a CI/CD pipeline using GitHub Actions with a self-hosted runner on your VM/server.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
  - [1. VM/Server Configuration](#1-vmserver-configuration)
  - [2. GitHub Actions Configuration](#2-github-actions-configuration)
  - [3. Self-hosted Runner Setup](#3-self-hosted-runner-setup)
- [How It Works](#how-it-works)
- [Troubleshooting](#troubleshooting)

## Prerequisites

- A GitHub repository
- A VM or server where you want to deploy your application
- Administrator access to both GitHub repository and the VM/server

## Setup Instructions

### 1. VM/Server Configuration

Generate and configure SSH keys for secure communication:

```bash
# Generate an Ed25519 SSH key pair
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add the SSH public key to your server's authorized_keys file
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
```

### 2. GitHub Actions Configuration

Set up required secrets in your GitHub repository:

1. Navigate to your repository on GitHub
2. Go to Settings > Secrets and Variables > Actions
3. Add the following repository secrets:

   - `VM_IP`: Your server's IP address
   - `VM_PORT`: SSH port (typically 22)
   - `VM_PRIVATE_KEY`: The entire content of your private SSH key file (id_ed25519)
   - `VM_USER`: Username for SSH login to your server

4. Create a workflow file (e.g., `.github/workflows/deploy.yml`):

#### Example Workflow

```yaml
name: Remote SSH

on:
  push:
    branches:
      - dev

jobs:
  deploy:
    runs-on: self-hosted

    steps:
      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1.2.2
        with:
          host: ${{ secrets.VM_IP }}
          username: ${{ secrets.VM_USER }}
          key: ${{ secrets.VM_PRIVATE_KEY }}
          port: ${{ secrets.VM_PORT }}
          script: |
            echo "=== Navigating to project directory ==="
            cd ~/projects/DAO-Proposal-Governance
            echo "Pulling latest changes from repository"
            git pull origin dev
            echo "Script executed successfully"
```

### 3. Self-hosted Runner Setup

Set up a GitHub Actions self-hosted runner on your VM/server:

1. In your GitHub repository, go to Settings > Actions > Runners
2. Click "New self-hosted runner"
3. Choose your desired runner image and architecture
4. Follow the provided instructions to download and configure the runner
5. To run the runner as a background service:

```bash
# Install the runner as a service
sudo ./svc.sh install

# Start the runner service
sudo ./svc.sh start
```

## How It Works

After completing the setup:

1. When you push changes to your configured branch (e.g., dev, main, etc.), GitHub Actions will automatically trigger the workflow
2. The self-hosted runner on your VM/server will execute the workflow
3. Your code will be deployed to the VM/server according to your workflow instructions

This CI/CD pipeline enables automatic deployment whenever changes are pushed to your repository.

## Troubleshooting

If you encounter issues:

- Verify that all secrets are correctly configured
- Check that the self-hosted runner is online and properly configured
- Review the GitHub Actions logs for detailed error information
- Ensure your VM/server has all necessary permissions and dependencies

## Maintainers

- **[Yamin Raad](https://github.com/Raad05)**
