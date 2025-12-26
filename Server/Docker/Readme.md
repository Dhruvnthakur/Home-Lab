# Docker Installation Guide (Ubuntu Server)

This document covers the step-by-step installation of **Docker Engine** on an **Ubuntu-based home server**.  
It is part of my home server setup journey, where Docker is used to run self-hosted services like dashboards, proxy managers, NAS tools, and more.

---

## 📌 Prerequisites

- Ubuntu Server 20.04 / 22.04 / 24.04
- User with `sudo` privileges
- Active internet connection
- Basic terminal access (SSH or local)

---

## 🧹 Step 1: Remove Old Docker Versions (Optional)

If Docker was installed previously, remove old packages to avoid conflicts:

```bash

sudo apt remove docker docker-engine docker.io docker-doc docker-compose docker-compose-v2 -y
```
## 🔄 Step 2: Update System Packages

```
sudo apt update && sudo apt upgrade -y
```
## 📦 Step 3: Install Required Dependencies

```
sudo apt install ca-certificates curl gnupg lsb-release -y
```
## 🔑 Step 4: Add Docker’s Official GPG Key

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
- Set correct permissions:
  ```
  sudo chmod a+r /etc/apt/keyrings/docker.gpg
  ```
## 📂 Step 5: Add Docker Repository

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```
## 🔄 Step 6: Update Package Index Again
```
sudo apt update

```
## 🐳 Step 7: Install Docker Engine
```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
## ✅ Step 8: Verify Docker Installation
```
docker --version
``` 
## 👤 Step 9: Run Docker Without sudo (Recommended)
- Add your user to the Docker group:
  ```
  sudo usermod -aG docker $USER
  ```
- Apply Changes:
  ```
  newgrp docker
  ``` 
- Test
  ```
  docker ps
  ```
## 🔁 Step 10: Enable Docker on Boot
```
sudo systemctl enable docker
sudo systemctl start docker
```
