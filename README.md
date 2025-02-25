# Kuzco Installation Guide

## Introduction to Kuzco Testnet

Kuzco.xyz Testnet is a globally distributed network of GPUs that enables AI inference tasks for large language models like Llama3, Mistral, and others. The network allows anyone with compatible GPU hardware to join and start earning $KZO points by contributing their computing power.

### What is Kuzco?
- A decentralized AI inference network
- Allows GPU owners to monetize their hardware by running AI inference tasks
- Supports popular AI models including Llama3, Mistral, and more
- Currently in testnet phase, with participants earning $KZO points

### Why Join Kuzco?
- Monetize your idle GPU resources
- Participate in building the decentralized AI infrastructure
- Earn $KZO points based on your contribution
- Be part of an early-stage project with future potential

### Requirements
- NVIDIA GPU with at least 16GB of VRAM
- 16GB system RAM minimum
- 30GB free disk space
- Reliable 100Mbps+ network connection

This guide provides detailed instructions for setting up Kuzco on both Ubuntu and Windows platforms.

## Table of Contents
- [Ubuntu Installation Guide (CLI Method)](#ubuntu-installation-guide)
- [Windows Installation Guide](#windows-installation-guide)
- [Managing Your Worker](#managing-your-worker)
- [Troubleshooting](#troubleshooting)

## Ubuntu Installation Guide

### Prerequisites
- Ubuntu OS (20.04 LTS or newer)
- NVIDIA GPU with at least 16GB of VRAM
- 16GB RAM minimum
- 30GB free disk space
- Reliable 100Mbps+ network connection

### Step 1: Update System
```bash
sudo apt update
sudo apt upgrade -y
```

### Step 2: Install NVIDIA Drivers
```bash
sudo apt install ubuntu-drivers-common -y
ubuntu-drivers devices
sudo ubuntu-drivers autoinstall
```

After installation, reboot your system:
```bash
sudo reboot
```

### Step 3: Verify GPU Driver Installation
```bash
nvidia-smi
```

You should see information about your GPU including driver version.

### Step 4: Install Kuzco CLI Method
```bash
curl -fsSL https://kuzco.xyz/install.sh | sh
```

### Step 7: Create and Run a Kuzco Worker

1. Register for an account at [kuzco.xyz/register](https://kuzco.xyz/register)
2. Verify your email and connect your Discord account (optional)
3. Login to your account on the CLI:
```bash
kuzco login
```

4. Create a worker:
```bash
kuzco worker create
```
Note your worker ID and code from the output.

5. Start your worker (Direct Method):
```bash
kuzco worker start --worker <your-worker-id> --code <your-worker-code>
```

### Step 8: Setting Up Persistent Kuzco Worker with tmux

1. Install tmux:
```bash
sudo apt install tmux -y
```

2. Create a new tmux session:
```bash
tmux new -s kuzco
```

3. Inside the tmux session, start your worker:
```bash
kuzco worker start --worker <your-worker-id> --code <your-worker-code>
```

4. Detach from tmux session by pressing `Ctrl+B` then `D`

5. To reconnect to your session later:
```bash
tmux attach -t kuzco
```

### Step 9: Setting Up Automatic Start on Boot (Optional)

Create a systemd service file:
```bash
sudo nano /etc/systemd/system/kuzco.service
```

Add the following content:
```
[Unit]
Description=Kuzco Worker Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/kuzco worker start --worker <your-worker-id> --code <your-worker-code>
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable kuzco.service
sudo systemctl start kuzco.service
```

Check service status:
```bash
sudo systemctl status kuzco.service
```

## Windows Installation Guide

### Prerequisites
- Windows 10/11
- NVIDIA GPU with at least 16GB of VRAM
- 16GB RAM minimum
- 30GB free disk space
- Reliable 100Mbps+ network connection

### Method 1: Desktop Application (Recommended)

1. Download the Kuzco Windows app from the [official website](https://kuzco.xyz)
2. Install the application
3. Register for an account
4. Follow the on-screen instructions to create and run a worker

### Method 2: Docker on Windows

1. Install Docker Desktop for Windows from [Docker's website](https://www.docker.com/products/docker-desktop)
2. Install the NVIDIA Driver for your GPU from [NVIDIA's website](https://www.nvidia.com/Download/index.aspx)
3. Install NVIDIA Container Toolkit for Windows
4. Register for a Kuzco account at [kuzco.xyz/register](https://kuzco.xyz/register)
5. Verify your email and connect your Discord account
6. Navigate to the "Workers" tab on the dashboard
7. Click "Create Worker" and select "Docker"
8. Copy the Docker command from the dashboard
9. Open PowerShell and run the Docker command:
```powershell
docker run --rm --runtime=nvidia --gpus all -e CACHE_DIRECTORY=/root/models -v ${HOME}/.kuzco/models:/root/models kuzcoxyz/amd64-ollama-nvidia-worker --worker <your-worker-id> --code <your-worker-code>
```

### Method 3: WSL (Windows Subsystem for Linux)

1. Enable WSL 2 and install Ubuntu from the Microsoft Store
2. Follow the Ubuntu installation guide above within your WSL environment

## Managing Your Worker

### Worker Management Commands

Check worker status:
```bash
kuzco worker status
```

View worker logs:
```bash
sudo kuzco worker logs
```

Restart worker:
```bash
sudo kuzco worker restart
```

Stop worker:
```bash
sudo kuzco worker stop
```

Clean worker data:
```bash
sudo kuzco clean --force
```

## Troubleshooting

### Common Issues and Solutions

1. **Error: Worker fails to initialize**
   - Solution: Check your GPU driver installation and make sure it's compatible with your GPU.

2. **Worker fails to start or crashes**
   - Check your GPU driver status: `nvidia-smi`
   - Check worker logs: `sudo kuzco worker logs`
   - Try stopping and restarting the worker
   - Try creating a new worker

3. **URL Parsing Errors**
   - Try running the worker without systemd: `kuzco worker start --worker <id> --code <code>`
   - Upgrade Kuzco: `kuzco upgrade`

4. **Worker gets disconnected frequently**
   - Ensure your internet connection is stable
   - Check if your firewall is blocking connections
   - Run the worker in a tmux session for persistence

### Getting Support

- Join the [Kuzco Discord](https://discord.gg/kuzco) for community support
- Open a ticket through the Discord server for technical assistance
- Monitor the #announcements channel for updates on service status
