# 🛠️ Ubuntu Dev Environment Setup (VirtualBox VM)

This guide helps you set up a development environment in an Ubuntu VM using VirtualBox, with tools like Docker, Terraform, GitHub CLI, AWS CLI, Nginx, and more.

---

## 📚 Table of Contents

1. [Install VirtualBox](#1-install-virtualbox)
2. [Create Ubuntu Virtual Machine](#2-create-ubuntu-virtual-machine)
3. [Install Guest Additions](#3-install-guest-additions)
4. [Install Development Tools](#4-install-development-tools)
   - [Docker](#docker--permissions-fix)
   - [Terraform](#terraform)
   - [Git + GitHub CLI](#git--github-cli)
   - [Nginx](#nginx)
   - [AWS CLI](#aws-cli)
5. [Verify Installation](#5-verify-installation)
6. [Screenshots](#6-screenshots)
7. [Troubleshooting](#7-troubleshooting)

---

## 1. Install VirtualBox

Download VirtualBox for your system:  
👉 [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

---

## 2. Create Ubuntu Virtual Machine

Download Ubuntu ISO:  
👉 [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

Create a VM with:

- **Name**: `ubuntu-dev`
- **Memory**: `4096 MB`
- **Disk**: `20 GB`, dynamically allocated
- Mount ISO under `Settings → Storage → Optical Drive`

---

## 3. Install Guest Additions

In the Ubuntu terminal:

```bash
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

Then:

```bash
sudo mkdir /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
sudo /mnt/cdrom/VBoxLinuxAdditions.run
```

Reboot:

```bash
sudo reboot
```

> Enable **Clipboard** and **Drag-and-Drop**:  
> VirtualBox → Settings → General → Advanced → Bidirectional

---

## 4. Install Development Tools

### Docker + Permissions Fix

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg |   sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]   https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### 🔓 Fix permissions

```bash
sudo usermod -aG docker $USER
newgrp docker
```

> Reboot the VM if needed.

---

### Terraform

```bash
sudo apt update
sudo apt install -y gnupg software-properties-common curl

curl -fsSL https://apt.releases.hashicorp.com/gpg |   sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg]   https://apt.releases.hashicorp.com $(lsb_release -cs) main" |   sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
sudo apt install -y terraform
```

---

### Git + GitHub CLI

```bash
sudo apt update
sudo apt install -y git curl

curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg |   sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg

sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg]   https://cli.github.com/packages stable main" |   sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null

sudo apt update
sudo apt install -y gh
```

---

### Nginx

```bash
sudo apt update
sudo apt install -y curl gnupg2 ca-certificates lsb-release ubuntu-keyring

curl -fsSL https://nginx.org/keys/nginx_signing.key | gpg --dearmor |   sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg > /dev/null

echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg]   http://nginx.org/packages/ubuntu $(lsb_release -cs) nginx" |   sudo tee /etc/apt/sources.list.d/nginx.list

sudo apt update
sudo apt install -y nginx
```

---

### AWS CLI

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```

---

## 5. Verify Installation

```bash
docker --version
docker compose version
terraform -v
git --version
gh --version
nginx -v
aws --version
```

---

## 6. Screenshots

![Screenshot of Installed Tools](/screenshots/Screenshot%20from%202025-04-22%2016-08-30.png)

---

## 7. Troubleshooting

### Docker: permission denied

If you get this error when running `docker ps`:

```
permission denied while trying to connect to the Docker daemon socket
```

Fix it with:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

> Then run `docker ps` again or reboot the VM.

---

## ✅ Done!

You now have a fully configured development environment with Docker, Terraform, Git, Nginx, AWS CLI, and more on your Ubuntu VM.
