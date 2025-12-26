# Glance Dashboard (Docker Compose Setup)

Glance is a **lightweight, self-hosted dashboard** used to aggregate system information, Docker containers, RSS feeds, search engines, and quick-access links into a single, clean web interface.

This repository documents my **home server Glance deployment** using **Docker Compose** for reliability, reproducibility, and long-term maintenance.

---

## ✨ Features

- Fast & minimal web dashboard
- Modular YAML configuration
- Multiple pages & widgets
- Docker container monitoring
- Server statistics
- Search engine selector
- Reverse-proxy friendly (Nginx Proxy Manager)

---

## 🧱 Tech Stack

- Ubuntu Server
- Docker Engine
- Docker Compose (v2 plugin)
- Glance

---

## Prerequisites

- Ubuntu Server 20.04+
- Docker installed and running
- Docker Compose v2
- Open port `8081`

Verify installation:

```bash
docker --version
docker compose version
```
## Directory Structure
```
/opt/glance/
├── docker-compose.yml
├── config/
│   ├── glance.yml
│   ├── theme.yml
│   └── home.yml
└── data/
```

## Configuration Files
- Glance.yml
- home.yml
- theme.yml
- server.yml

## Docker Compose File
- Create docker-compose.yml:
```
version: "3.8"

services:
  glance:
    image: glanceapp/glance:latest
    container_name: glance
    restart: unless-stopped
    ports:
      - "8081:8080"
    volumes:
      - ./config/glance.yml:/app/config/glance.yml
      - ./config/theme.yml:/app/config/theme.yml
      - ./config/home.yml:/app/config/home.yml
      - /var/run/docker.sock:/var/run/docker.sock
```
## Start Glance
- From /opt/glance:
```
docker compose up -d
```
## Access Dashboard
- Open in browser:
```
http://<server-ip>:8081
```
## Updating Configuration
-To restart manually:
```
docker compose restart
```
## Authentication (Optional)
- Add to config/glance.yml:
```
authentication:
  type: basic
  username: admin
  password: strongpassword
```
## Stop & Remove
```
docker compose down
```


















