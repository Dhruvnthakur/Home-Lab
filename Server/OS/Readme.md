# 🧠 My HomeLab Journey

> **Work in Progress** - Documenting the transformation of an ancient PC into a full-featured home server.

---

## 📂 Table of Contents
- [OS Selection & Installation](#os-selection--installation)
- [Hardware Overview](#hardware-overview)
- [Roadmap](#roadmap)
- [Tech Stack](#tech-stack)

---

## 🖥️ OS Selection & Installation

### Why Ubuntu Server 24.04?

**Ubuntu Server 24.04** was chosen for this homelab because:

- **Ultra-lightweight**: No GUI overhead makes it perfect for my ancient PC's low specs (limited RAM/CPU).
- **Server-optimized**: Lacks desktop bloat, consuming ~300-500MB RAM at idle vs 2GB+ for Ubuntu Desktop.
- **Long-term support**: LTS until 2029 with security updates.
- **Massive community**: Perfect for troubleshooting homelab issues.

**Ubuntu Desktop → ❌ Too heavy for old hardware**  
**Ubuntu Server → ✅ Perfect balance of features + performance**

### Step 0: Create Bootable USB

1. **Download official ISO**:

- Ubuntu Server 24.04 LTS: https://ubuntu.com/download/server
- Direct Download [Ubuntu](https://ubuntu.com/download/server/thank-you?version=24.04.3&architecture=amd64&lts=true)


2. **Flash bootable drive** (Windows/Mac/Linux):
- **Recommended**: [Rufus](https://rufus.ie/) (Windows) or `dd` (Linux/Mac) [I personally had problem using Rufus] 
- **Balena Etcher**: Cross-platform GUI alternative (Recommended)

**Linux/Mac command**:
  ```
  sudo dd if=ubuntu-24.04-live-server-amd64.iso of=/dev/sdX bs=4M status=progress && sync
  ```
*Replace `/dev/sdX` with your USB drive (check with `lsblk`)*

### Step 1: Boot & Install

1. Insert USB → Restart PC → Enter BIOS (F2/Del) → Set USB as first boot device
2. Select **"Install Ubuntu Server"**
3. Follow installer until **Storage Configuration**:

#### ⚠️ Important: Skip LVM (Logical Volume Manager)
[ ] Use an LVM group with the new Ubuntu installation


**Why skip LVM for personal homelab?**
- **Unnecessary complexity**: Single disk → no need for snapshots/resizing
- **Performance overhead**: Slight I/O penalty on old hardware
- **Simpler recovery**: Direct ext4 partition = easier backups/rescue
- **LVM better for**: Enterprise multi-disk arrays, not single-drive homelabs

**Choose**: `Use entire disk and set up LVM` → **Tab to `<Continue>` without checking LVM box**

### Step 2: Essential Post-Install Setup

```
sudo apt update && sudo apt upgrade -y
sudo apt install openssh-server -y
sudo systemctl enable ssh --now
sudo ss -tuln | grep :22 # Verify
```


**Check "Install OpenSSH server"** during installer for immediate remote access.

### Step 3: First Boot & Hardening

- Create non-root user (if not already created)
  
  ```
  sudo adduser $USER
  sudo usermod -aG sudo $USER
  ```
- Disable root login for security

  ```
  sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
  sudo systemctl restart ssh
  ```
- Set custom hostname

  ```
  sudo hostnamectl set-hostname homelab-server
  ```

**Test remote access:**
- On your daily use PC / Laptop

  ```
  ssh yourusername@<SERVER-IP>
  ```
- SERVER-IP can be found with bashing on Server

  ```
  ip a
  ```


---

## Hardware Overview

| Component | Specs | Notes |
|-----------|-------|-------|
| CPU | Ancient Intel/AMD | Single-core OK |
| RAM | 2-4GB | Docker-ready |
| Storage | 500GB-1TB HDD | Plex + backups |
| NIC | 1Gbps Ethernet | Tailscale ready |

---

## Roadmap

1. ✅ Ubuntu Server 24.04
2. 🔄 Docker + containers
3. 🔄 Tailscale VPN
4. 🔄 Plex/Jellyfin
5. 🔄 OpenStack Glance

---

## Tech Stack
-OS: Ubuntu Server 24.04 LTS
-Containers: Docker
-VPN: Tailscale
-Media: Plex/Jellyfin
-Storage: OpenStack Glance


**🚀 Next: Docker installation!** ⭐ Star to follow along!

