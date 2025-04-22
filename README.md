# Local Development Environment Setup (Ubuntu VM on VirtualBox)

This guide explains how to set up a full local development environment using an Ubuntu virtual machine (VM) inside VirtualBox and install all necessary tools for modern development.

## 1. Install VirtualBox

Download and install VirtualBox for your operating system:  
[VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

## 2. Create Ubuntu Virtual Machine

Download the Ubuntu ISO from:  
[Ubuntu Download](https://ubuntu.com/download/desktop)

Open VirtualBox and create a new VM:

- **Name**: `ubuntu-dev`
- **Type**: `Linux`
- **Version**: `Ubuntu (64-bit)`
- **Memory**: at least `4096 MB`
- **Virtual hard disk**: at least `20 GB` (dynamically allocated)

In VM Settings:

- Under **Storage**, mount the downloaded ISO as the optical drive.
- Start the VM and follow the Ubuntu installation steps.

## 3. Install VirtualBox Guest Additions (Enable Clipboard and Drag-and-Drop)

Inside the VM, open a terminal and run the following commands:

```
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

From the VirtualBox Menu:
Go to Devices → Insert Guest Additions CD Image

Then run the following commands:

```
sudo mkdir /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
sudo /mnt/cdrom/VBoxLinuxAdditions.run
```

After the installation completes, reboot the VM.

Enable Bidirectional Clipboard and Drag-and-Drop:

Go to VirtualBox → VM Settings → General → Advanced

## 4. Install Development Tools on Ubuntu VM

### Docker

```
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
 sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
 "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
 https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
 sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Optional Docker Compose binary (if needed separately):

```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
 -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### Terraform

```
sudo apt update
sudo apt install -y gnupg software-properties-common curl

curl -fsSL https://apt.releases.hashicorp.com/gpg | \
 sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
sudo apt install -y terraform
terraform -v

```

### Git and GitHub CLI

```

sudo apt update
sudo apt install -y git curl

curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | \
 sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg

sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] \
https://cli.github.com/packages stable main" | \
sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null

sudo apt update
sudo apt install -y gh

```

### Nginx (from official repository)

To install Nginx, run:

```

sudo apt update
sudo apt install -y curl gnupg2 ca-certificates lsb-release ubuntu-keyring
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null

echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/ubuntu $(lsb_release -cs) nginx" | \
 sudo tee /etc/apt/sources.list.d/nginx.list

sudo apt update
sudo apt install -y nginx
nginx -v

```

### AWS CLI v2

```

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install -y unzip
unzip awscliv2.zip
sudo ./aws/install
aws --version

```

## 5. Verify Installation

Check versions to ensure all tools were installed correctly:

```

docker --version
docker compose version
terraform -v
git --version
gh --version
nginx -v
aws --version

```

## 6. Screenshots Showing Tool Versions

![Tool Versions](/screenshots/Screenshot%20from%202025-04-22%2016-08-30.png)
