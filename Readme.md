# Self-Hosted n8n with Traefik Reverse Proxy

[![Docker](https://img.shields.io/badge/Docker-✓-blue)](https://www.docker.com)
[![Traefik](https://img.shields.io/badge/Traefik-v3.1.5-blue)](https://traefik.io)
[![n8n](https://img.shields.io/badge/n8n-✓-blue)](https://n8n.io)

A production-ready Docker deployment of n8n workflow automation secured behind Traefik with automatic HTTPS via Let's Encrypt.

## Table of Contents
- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Security](#-security)
- [Maintenance](#-maintenance)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

## 🌟 Features
- **Automatic HTTPS** with Let's Encrypt certificates
- **Cloudflare DNS challenge** for certificate issuance
- **PostgreSQL** database for persistent workflow storage
- **Docker Compose** deployment
- **Traefik reverse proxy** with automatic service discovery
- **HTTP to HTTPS redirect** for all traffic

## 🛠 Prerequisites
- Linux server with:
  - Docker installed
  - Docker Compose installed
- Domain name pointing to your server (e.g., `n8n.isendglobal.com`)
- Cloudflare account with API token
- Ports 80 and 443 accessible on your server

## 🚀 Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/traefik-n8n.git
   cd traefik-n8n

2. **Set up directories**:
   ```bash
   mkdir -p traefik/{acme,config}
   chmod 600 traefik/acme
   
3. **Create Environment files**:
   ```bash
   echo "CLOUDFLARE_API_TOKEN=your_cloudflare_token_here" > .env
   
4. **Start the services**:
   ```bash
   docker-compose up -d
   
5. **Access n8n**:
   # Open https://yourdomain.com in your browser


## ⚙️ Configuration
File Structure
Copy

.
├── docker-compose.yml        # Main deployment configuration
├── .env                     # Environment variables
└── traefik/
    ├── traefik.yaml         # Traefik configuration
    ├── acme/                # Certificate storage
    └── config/              # Additional config files

Key Components

    Traefik (traefik/traefik.yaml):

        Manages SSL certificates

        Handles HTTP to HTTPS redirects

        Provides secure access to n8n

    n8n:

        Runs on port 5678 internally

        Accessible externally via https://n8n.isendglobal.com

        Stores workflows in PostgreSQL

    PostgreSQL:

        Persistent database storage

        Automatic health checks

🔒 Security

    Encrypted connections: All traffic forced to HTTPS

    Certificate auto-renewal: Let's Encrypt certificates renew automatically

    Isolated network: Services communicate through private Docker network

    Secure storage: Database credentials encrypted in transit

    Limited exposure: Only necessary ports exposed

🔄 Maintenance
Update Containers
bash
Copy

docker-compose pull && docker-compose up -d

Backup Data
bash
Copy

# Backup n8n data
docker run --rm -v n8n-data:/source -v $(pwd):/backup alpine \
  tar cvf /backup/n8n-backup-$(date +%Y%m%d).tar /source

# Backup PostgreSQL data
docker run --rm -v postgres-data:/source -v $(pwd):/backup alpine \
  tar cvf /backup/postgres-backup-$(date +%Y%m%d).tar /source

View Logs
bash
Copy

# View Traefik logs
docker-compose logs -f traefik

# View n8n logs
docker-compose logs -f n8n

🚨 Troubleshooting
Symptom	Solution
Certificate errors	Run rm traefik/acme/cloudflare.json and restart
n8n not accessible	Check docker-compose ps for running containers
404 errors	Verify Traefik labels and network configuration
Database issues	Check PostgreSQL logs with docker-compose logs postgres
📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

