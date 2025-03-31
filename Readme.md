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
