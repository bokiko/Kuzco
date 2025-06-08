# 🚀 Kuzco: Inference.net Epoch 3 - Complete Setup Guide


**Follow [@bokiko_io](https://twitter.com/bokiko_io) for updates and crypto mining guides**

---

## 📑 Quick Navigation

| Section | What You'll Do |
|---------|----------------|
| [🎯 Start Here](#-start-here) | Check if you can run this |
| [💻 Ubuntu Setup](#-ubuntu-setup) | Linux installation (Recommended) |
| [🪟 Windows Setup](#-windows-setup) | Windows installation (3 methods) |
| [⚡ Get Running](#-get-running) | Start earning immediately |
| [💰 Rewards Explained](#-how-rewards-work) | Understand the money |
| [📅 Timeline](#-epoch-3-roadmap) | What's coming next |
| [🔧 Troubleshooting](#-troubleshooting) | Fix common issues |

---

## 🎯 Start Here

### What is Inference.net?

**The world's largest decentralized GPU network** where you rent out your graphics card to run AI models and get paid in crypto. Think of it as Airbnb for GPUs.

### ✅ Do You Qualify?

**Your GPU needs:**
- **NVIDIA only** (AMD coming soon - contact support@inference.net)
- **8GB+ VRAM minimum** (16GB+ recommended for better earnings)
- **Popular cards**: RTX 3060 (12GB), RTX 3070, RTX 3080, RTX 3090, RTX 4070, RTX 4080, RTX 4090

**Your system needs:**
- 16GB+ RAM
- 50GB+ free storage
- Stable internet (100Mbps+)
- Ubuntu Linux OR Windows 10/11

**💡 Quick Check:** Run `nvidia-smi` in terminal. See your GPU? You're good to go!

### 🔥 What's New in Epoch 3?

- **Auto-updates**: No more manual updating
- **Solana integration**: Real crypto payments ($INT-DEV tokens)
- **Stake-weighted rewards**: More stake = more jobs = more money
- **Strict GPU detection**: Only verified GPUs allowed (better stability)

---

## 💻 Ubuntu Setup

*Most popular method - better performance, easier management*

### Step 1: Prepare Your System

```bash
# Update everything
sudo apt update && sudo apt upgrade -y

# Install essentials
sudo apt install curl wget tmux nano ubuntu-drivers-common -y
```

### Step 2: Install NVIDIA Drivers

```bash
# Check your GPU
nvidia-smi

# If that fails, install drivers:
ubuntu-drivers devices
sudo ubuntu-drivers autoinstall
sudo reboot

# Verify after reboot
nvidia-smi
```

**✅ Success:** You should see your GPU name, temperature, and memory usage.

### Step 3: Install Inference.net

```bash
# One command installation
curl -fsSL https://devnet.inference.net/install.sh | sh
```

### Step 4: Create Your Account

1. **Sign up**: [devnet.inference.net](https://devnet.inference.net)
2. **Verify email** (check spam folder)
3. **Link Solana wallet** (CRITICAL - do this before June 13th for tokens!)
4. **Join Discord** (optional): [discord.gg/kuzco](https://discord.gg/kuzco)

### Step 5: Start Earning

```bash
# Login
inference login

# Create worker
inference worker create

# Copy the worker ID and code it gives you!

# Start earning (replace with your actual values)
inference worker start --worker <your-worker-id> --code <your-worker-code>
```

### Step 6: Keep It Running 24/7

**Option A: Using tmux (Easy to monitor)**
```bash
# Create persistent session
tmux new -s inference-node

# Start worker inside tmux
inference worker start --worker <your-worker-id> --code <your-worker-code>

# Detach: Press Ctrl+B, then D
# Reconnect anytime: tmux attach -t inference-node
```

**Option B: Auto-start on boot (Set and forget)**
```bash
# Create service file
sudo nano /etc/systemd/system/inference.service
```

Paste this (replace with your worker ID and code):
```ini
[Unit]
Description=Inference.net Worker
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

Enable it:
```bash
sudo systemctl daemon-reload
sudo systemctl enable inference.service
sudo systemctl start inference.service

# Check status
sudo systemctl status inference.service
```

**✅ You're Done!** Your GPU is now earning crypto 24/7.

---

## 🪟 Windows Setup

### Method 1: Desktop App (Easiest)

1. **Download**: [devnet.inference.net](https://devnet.inference.net) → Download Windows app
2. **Install** → Follow wizard
3. **Sign up** → Verify email
4. **Link Solana wallet** (before June 13th!)
5. **Create worker** → Follow app instructions
6. **Start earning** → Click "Start Worker"

**✅ Done!** Easiest method for beginners.

### Method 2: Docker (Advanced Users)

**Prerequisites:**
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [NVIDIA drivers](https://www.nvidia.com/Download/index.aspx)
- NVIDIA Container Toolkit

**Steps:**
1. Create account at [devnet.inference.net](https://devnet.inference.net)
2. Link Solana wallet
3. Go to "Workers" → "Create Worker" → "Docker"
4. Copy the command and run in PowerShell:

```powershell
docker run --rm --runtime=nvidia --gpus all -e CACHE_DIRECTORY=/root/models -v ${HOME}/.inference/models:/root/models inference.net/worker --worker <your-worker-id> --code <your-worker-code>
```

### Method 3: WSL (Linux on Windows)

**Why use this?** Better performance than Windows app, easier than Docker.

**Setup WSL:**
```powershell
# Run as Administrator in PowerShell
wsl --install

# Restart computer
# Install Ubuntu 22.04 from Microsoft Store
```

**Install Inference.net:**
```bash
# Open Ubuntu terminal
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget tmux nano -y

# Install Inference.net
curl -fsSL https://devnet.inference.net/install.sh | sh

# Follow Ubuntu steps 4-6 above
```

**💡 Tip:** WSL gives you Linux performance on Windows!

---

## ⚡ Get Running

### Essential Commands

**Check everything is working:**
```bash
inference worker status
```

**View live earnings/logs:**
```bash
inference worker logs
```

**Restart if stuck:**
```bash
inference worker restart
```

**Stop completely:**
```bash
inference worker stop
```

**Create new worker:**
```bash
inference worker create
```

**Clean reset:**
```bash
inference worker clean
```

**Update client:**
```bash
inference version  # Check current version
# Auto-updates enabled - no manual updates needed!
```

**Get help:**
```bash
inference --help
inference worker --help
```

### 🎯 Pro Tips

- **Monitor GPU**: `nvidia-smi -l 1` (live GPU monitoring)
- **Check disk space**: `df -h` (models take space)
- **Temperature**: Keep GPU under 80°C for longevity
- **Internet**: Stable connection = more jobs = more money

---

## 💰 How Rewards Work

### Two Types of Rewards

**$INT Points (Immediate)**
- Earned for every AI job processed
- Real-time accumulation
- Bonus points for community contributions

**$INT-DEV Tokens (Real Crypto)**
- Distributed daily at midnight UTC
- Based on your stake and performance
- Trade on Solana DEX

### The Stake System

**Higher Stake = More Jobs = More Money**

Your earnings depend on:
- **Your GPU power** (VRAM amount)
- **Your stake** (INT tokens you hold)
- **Your reputation** (uptime and performance)
- **Network demand** (busy periods = more earnings)

**Example:**
- GPU: RTX 4090 (24GB VRAM)
- Stake: 100,000 INT tokens
- High uptime: 99%+
- Result: Premium job allocation

### Delegation System

**Don't have enough tokens?** Others can delegate their tokens to your worker and share earnings. Higher combined stake = more jobs for everyone.

---

## 📅 Epoch 3 Roadmap

| Date | What Happens | Impact |
|------|-------------|---------|
| **✅ June 6** | Network Launch | • Auto-updates active<br>• Enhanced detection<br>• New unified engine |
| **🔥 June 13** | Economic Layer | • **$INT-DEV token airdrop**<br>• Staking goes live<br>• Delegation enabled |
| **📈 June 20** | Bonus Features | • Community rewards<br>• Enhanced analytics<br>• Performance tracking |
| **🚀 Late June** | Advanced Features | • Full reputation system<br>• Performance routing<br>• Pool management |

**⚠️ IMPORTANT:** Link your Solana wallet before June 13th to receive token airdrop!

---

## 🔧 Troubleshooting

### GPU Not Detected

**Problem:** `Failed to detect GPUs`

**Solutions:**
```bash
# Check GPU
nvidia-smi

# If fails, reinstall drivers
sudo ubuntu-drivers autoinstall
sudo reboot

# Verify permissions
sudo usermod -a -G video $USER
logout  # and login again
```

### AMD GPU Users

**Current Status:** AMD GPUs not supported yet in Epoch 3.

**What to do:**
1. Contact support@inference.net
2. Join Discord: [discord.gg/kuzco](https://discord.gg/kuzco)
3. Request AMD support with your GPU model

**Your GPU specs:**
```bash
lspci | grep -i amd
clinfo  # Check OpenCL support
```

### Connection Issues

**Problem:** Worker keeps disconnecting

**Solutions:**
```bash
# Test connection
ping devnet.inference.net

# Check firewall
sudo ufw status

# Restart networking
sudo systemctl restart networking

# Clean restart worker
inference worker stop
inference worker clean
inference worker start --worker <id> --code <code>
```

### Performance Issues

**Problem:** Low earnings or slow processing

**Check:**
```bash
# GPU temperature (keep under 80°C)
nvidia-smi

# Memory usage
free -h

# Disk space (need 50GB+ free)
df -h

# Network speed
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python3 -
```

### Log Analysis

**See what's happening:**
```bash
# Worker logs
inference worker logs

# Follow live logs
inference worker logs --follow

# System service logs
journalctl -u inference.service -f
```

### Emergency Reset

**Nuclear option if everything breaks:**
```bash
# Stop everything
inference worker stop

# Clean all data
inference worker clean --force

# Reinstall
curl -fsSL https://devnet.inference.net/install.sh | sh

# Start fresh
inference login
inference worker create
inference worker start --worker <new-id> --code <new-code>
```

---

## 🆘 Get Help

### Community Support
- **Discord**: [discord.gg/kuzco](https://discord.gg/kuzco) (fastest help)
- **Twitter**: [@bokiko_io](https://twitter.com/bokiko_io) (updates and tips)
- **Email**: support@inference.net

### Documentation
- **Official Docs**: [docs.devnet.inference.net](https://docs.devnet.inference.net)
- **Dashboard**: [devnet.inference.net](https://devnet.inference.net)
- **This Guide**: Always updated with latest info

### Before Asking for Help

**Include this info:**
```bash
# Your system info
uname -a
nvidia-smi
inference version

# Your error logs
inference worker logs | tail -50
```

---

**🎉 Congratulations!** Your GPU is now part of the world's largest decentralized AI network. Happy mining!

*Guide by [@bokiko_io](https://twitter.com/bokiko_io) - Updated June 7, 2025*
