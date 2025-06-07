# Inference.net Epoch 3 - Complete Installation Guide

## 📋 Overview

Inference.net Epoch 3 launched on June 6th, 2025, introducing significant protocol changes including Solana integration, stake-weighted routing, and automatic node updates. This guide provides comprehensive instructions for setting up your node to participate in the decentralized AI inference network.

### What is Inference.net?

Inference.net is a globally distributed network of GPUs that enables AI inference tasks for large language models. The network allows GPU owners to contribute computing power and earn $INT points and $INT-DEV tokens through a stake-weighted system.

### Key Features of Epoch 3

- **Automatic Node Updates**: Auto-update system activation for all node types with health checks and automatic rollback
- **Unified Inference Engine**: Single container that automatically detects hardware specifications and selects optimal inference engine
- **Stake-Weighted Job Routing**: Economic incentives for reliable operation with priority scoring
- **Solana Integration**: On-chain staking protocol with $INT-DEV tokens
- **Enhanced GPU Detection**: Strict GPU detection requirements - only properly identified GPUs can join the network

---

## 🛠 Hardware Requirements

### Minimum System Requirements

- **GPU**: NVIDIA GPU with 8GB+ VRAM (16GB+ recommended)
- **RAM**: 16GB system memory minimum (32GB+ recommended)
- **Storage**: 50GB+ free disk space for models and cache
- **Network**: Stable broadband connection (100Mbps+ recommended)
- **OS**: Ubuntu 20.04 LTS or newer / Windows 10/11

### Supported GPUs

- **High Performance**: RTX 4090, RTX 4080, A100, H100
- **Mid-Range**: RTX 3090, RTX 3080, RTX 4070, A6000
- **Entry Level**: RTX 3070, RTX 4060 Ti (16GB), RTX 3060 (12GB)

**Note**: Starting June 6th, only GPUs that can be properly identified by the detection system will be permitted to join the network

---

## 🐧 Ubuntu Installation Guide

### Step 1: System Preparation

Update your system and install essential packages:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget tmux nano ubuntu-drivers-common -y
```

### Step 2: NVIDIA Driver Installation

Check available drivers and install:

```bash
# Check your GPU
lspci | grep -i nvidia

# See available drivers
ubuntu-drivers devices

# Install recommended driver
sudo ubuntu-drivers autoinstall

# Reboot system
sudo reboot
```

Verify installation:

```bash
nvidia-smi
```

You should see your GPU information and driver version.

### Step 3: Install Inference.net Node

Install the unified node software:

```bash
curl -fsSL https://devnet.inference.net/install.sh | sh
```

### Step 4: Account Setup

1. **Register Account**: Visit [devnet.inference.net](https://devnet.inference.net) and create an account
2. **Link Solana Wallet**: Link your Solana wallet on the dashboard to your Inference.net account before June 13th
3. **Verify Email**: Complete email verification
4. **Connect Discord** (Optional): Link Discord for community updates

### Step 5: Node Authentication

Login to your account:

```bash
inference login
```

### Step 6: Create and Start Worker

Create a new worker:

```bash
inference worker create
```

**Important**: Save the worker ID and code displayed in the output.

Start your worker:

```bash
inference worker start --worker <your-worker-id> --code <your-worker-code>
```

### Step 7: Set Up Persistent Operation with tmux

Install and configure tmux for background operation:

```bash
# Create tmux session
tmux new -s inference

# Inside tmux, start your worker
inference worker start --worker <your-worker-id> --code <your-worker-code>

# Detach from session: Press Ctrl+B, then D
# Reconnect later: tmux attach -t inference
```

### Step 8: Auto-Start Configuration (Optional)

Create systemd service for automatic startup:

```bash
sudo nano /etc/systemd/system/inference.service
```

Add the following content:

```ini
[Unit]
Description=Inference.net Worker Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/inference worker start --worker <your-worker-id> --code <your-worker-code>
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable inference.service
sudo systemctl start inference.service
sudo systemctl status inference.service
```

---

## 🪟 Windows Installation Guide

### Method 1: Desktop Application (Recommended)

1. **Download**: Visit [devnet.inference.net](https://devnet.inference.net) and download the Windows desktop app
2. **Install**: Run the installer and follow the setup wizard
3. **Register**: Create account and verify email
4. **Link Wallet**: Connect your Solana wallet in the dashboard
5. **Configure**: Follow the app's guided setup for worker creation
6. **Run**: Start your worker through the desktop interface

### Method 2: Docker Method (Advanced)

#### Prerequisites

- Install [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop)
- Install latest [NVIDIA GPU drivers](https://www.nvidia.com/Download/index.aspx)
- Install NVIDIA Container Toolkit

#### Setup Steps

1. **Register Account**: Create account at [devnet.inference.net](https://devnet.inference.net)
2. **Link Solana Wallet**: Connect wallet in the dashboard
3. **Create Worker**: Navigate to "Workers" tab → "Create Worker" → Select "Docker"
4. **Copy Command**: Copy the provided Docker command from dashboard
5. **Run Container**:

```powershell
docker run --rm --runtime=nvidia --gpus all -e CACHE_DIRECTORY=/root/models -v ${HOME}/.inference/models:/root/models inference.net/worker --worker <your-worker-id> --code <your-worker-code>
```

---

## 📊 Node Management Commands

### Essential Commands

Check worker status:
```bash
inference worker status
```

View worker logs:
```bash
inference worker logs
```

Stop worker:
```bash
inference worker stop
```

Restart worker:
```bash
inference worker restart
```

Check node version:
```bash
inference version
```

### Monitoring Your Node

View real-time performance:
```bash
# Check GPU usage
nvidia-smi

# Monitor system resources
htop

# Check network connections
inference worker stats
```

---

## 💰 Understanding the Reward System

### Dual Token System

**$INT Points (Off-chain)**:
- Accumulated in real-time as jobs are processed
- Calculated based on computational work performed
- May be awarded for non-compute contributions (guides, community help)

**$INT-DEV Tokens (On-chain)**:
- Distributed daily at midnight UTC
- Based on stake-weighted job completion
- Required for staking to receive job allocations

### Stake-Weighted Routing

Each instance receives a priority score that determines job probability based on Device VRAM, Total INT Stake, Reputation Weight, and network parameter k

**Priority Score Factors**:
- **VRAM Normalization**: Stake distributed across your total VRAM
- **Total Stake**: Operator-owned + delegated tokens
- **Reputation**: Performance-based multiplier (coming later in Epoch 3)
- **Network Parameter k**: Adjusts based on utilization

---

## 🔧 Troubleshooting

### Common Issues

**GPU Not Detected**:
```bash
# Check GPU status
nvidia-smi

# Verify driver installation
nvidia-docker --version

# Update drivers
sudo ubuntu-drivers autoinstall && sudo reboot
```

**Worker Connection Issues**:
```bash
# Check network connectivity
ping devnet.inference.net

# Restart worker
inference worker restart

# Check firewall settings
sudo ufw status
```

**Performance Issues**:
```bash
# Monitor GPU temperature
nvidia-smi -l 1

# Check disk space
df -h

# Monitor memory usage
free -h
```

**Auto-Update Problems**:
The auto-update mechanism performs health checks and automatically rollbacks to previous stable version if update fails

### Log Analysis

View detailed logs:
```bash
# Worker logs
inference worker logs --follow

# System logs
journalctl -u inference.service -f

# GPU logs
nvidia-smi dmon
```

### Getting Help

**Community Support**:
- Join [Discord](https://discord.gg/kuzco) for real-time help
- Monitor #announcements for network updates
- Open support tickets through Discord

**Documentation**:
- Official docs: [docs.devnet.inference.net](https://docs.devnet.inference.net)
- Dashboard: [devnet.inference.net](https://devnet.inference.net)

---

## 📅 Epoch 3 Timeline

| Date | Feature | Description |
|------|---------|-------------|
| **June 6, 2025** | Network Upgrade | Auto-update system, enhanced GPU detection, unified inference engine, simplified deployment |
| **June 13, 2025** | Economic Layer | $INT-DEV token airdrop, staking protocol launch, stake-weighted routing, delegation functionality |
| **June 20, 2025** | Extended Features | Bonus point system, additional earning opportunities, enhanced monitoring |
| **Late June 2025** | Advanced Features | Full reputation scoring, performance-based routing, slashing mechanism testing |

---

## ⚡ Quick Start Summary

1. **Install**: `curl -fsSL https://download.inference.net/install.sh | sh`
2. **Register**: Create account at [devnet.inference.net](https://devnet.inference.net)
3. **Link Wallet**: Connect Solana wallet before June 13th
4. **Login**: `inference login`
5. **Create Worker**: `inference worker create`
6. **Start**: `inference worker start --worker <id> --code <code>`
7. **Monitor**: `inference worker status`

---

## 📞 Support & Resources

- **Documentation**: [docs.devnet.inference.net](https://docs.devnet.inference.net)
- **Dashboard**: [devnet.inference.net](https://devnet.inference.net)
- **Discord Community**: [discord.gg/kuzco](https://discord.gg/kuzco)
- **Status Updates**: Follow #announcements on Discord

*Last Updated: June 7, 2025 - Epoch 3 Launch*
