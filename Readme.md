# Self-Hosted n8n with Traefik Reverse Proxy

[![Docker](https://img.shields.io/badge/Docker-✓-blue)](https://www.docker.com)
[![Traefik](https://img.shields.io/badge/Traefik-v3.1.5-blue)](https://traefik.io)
[![n8n](https://img.shields.io/badge/n8n-✓-blue)](https://n8n.io)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue)](https://www.postgresql.org/)
[![Let's Encrypt](https://img.shields.io/badge/Let's_Encrypt-✓-brightgreen)](https://letsencrypt.org/)

A production-ready Docker deployment of [n8n](https://n8n.io) workflow automation secured behind Traefik with automatic HTTPS via Let's Encrypt. This setup provides a robust, scalable environment for running your workflow automations securely.

## 🌟 Features

- **Automatic HTTPS** with Let's Encrypt certificates
- **Cloudflare DNS challenge** for certificate issuance
- **PostgreSQL** database for persistent workflow storage
- **Docker Compose** for simple deployment and management
- **Traefik reverse proxy** with automatic service discovery
- **HTTP to HTTPS redirect** for enhanced security
- **Container health checks** for reliability
- **Volume-based persistence** for data durability
- **Automatic backups** support

## 🛠 Prerequisites

- Linux server with:
  - Docker Engine (v20.10+)
  - Docker Compose (v2.0+)
  - 2GB RAM minimum (4GB recommended)
  - 10GB free disk space
- Domain name pointing to your server (e.g., `n8n.isendglobal.com`)
- Cloudflare account with API token that has Zone:DNS:Edit permissions
- Ports 80 and 443 accessible on your server

## 🚀 Installation

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/traefik-n8n.git
cd traefik-n8n
```

### 2. Set up directories

```bash
mkdir -p traefik/{acme,config}
chmod 600 traefik/acme
```

### 3. Create environment files

```bash
cp .env.example .env
# Edit .env and add your Cloudflare API token and other configuration
nano .env
```

### 4. Start the services

```bash
docker-compose up -d
```

### 5. Access n8n

Open `https://yourdomain.com` in your browser. The initial setup will guide you through creating an admin account.

## ⚙️ Configuration

### File Structure

```
.
├── docker-compose.yml        # Main deployment configuration
├── .env                      # Environment variables
├── .env.example              # Example environment configuration
└── traefik/
    ├── traefik.yaml         # Traefik configuration
    ├── acme/                # Certificate storage
    └── config/              # Additional config files
```

### Key Components

#### Traefik (`traefik/traefik.yaml`)

- Manages SSL certificates
- Handles HTTP to HTTPS redirects
- Provides secure access to n8n
- Monitors Docker for new services

#### n8n

- Runs on port 5678 internally
- Accessible externally via https://yourdomain.com
- Stores workflows in PostgreSQL
- Custom environment variables configurable in `.env`

#### PostgreSQL

- Persistent database storage
- Automatic health checks
- Data stored in Docker volume

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `CLOUDFLARE_API_TOKEN` | Your Cloudflare API token | |
| `DOMAIN` | Your domain (e.g., n8n.example.com) | |
| `POSTGRES_USER` | PostgreSQL username | n8n |
| `POSTGRES_PASSWORD` | PostgreSQL password | | 
| `POSTGRES_DB` | PostgreSQL database name | n8n |
| `N8N_BASIC_AUTH_ACTIVE` | Enable basic auth for n8n | false |
| `N8N_BASIC_AUTH_USER` | Basic auth username | | 
| `N8N_BASIC_AUTH_PASSWORD` | Basic auth password | |

## 🔒 Security

- **Encrypted connections**: All traffic forced to HTTPS
- **Certificate auto-renewal**: Let's Encrypt certificates renew automatically
- **Isolated network**: Services communicate through private Docker network
- **Secure storage**: Database credentials encrypted in transit
- **Limited exposure**: Only necessary ports exposed
- **Optional basic auth**: Additional layer of authentication

### Security Recommendations

1. Use strong passwords for all credentials
2. Enable basic authentication for an extra layer of security
3. Keep Docker and all containers updated
4. Implement a firewall (e.g., ufw) on your host
5. Set up regular data backups

## 🔄 Maintenance

### Update Containers

```bash
docker-compose pull && docker-compose up -d
```

### Backup Data

```bash
# Backup n8n data
docker run --rm -v n8n-data:/source -v $(pwd):/backup alpine \
  tar cvf /backup/n8n-backup-$(date +%Y%m%d).tar /source

# Backup PostgreSQL data
docker run --rm -v postgres-data:/source -v $(pwd):/backup alpine \
  tar cvf /backup/postgres-backup-$(date +%Y%m%d).tar /source
```

### Scheduled Backups

Create a cron job to run backups regularly:

```bash
# Add to crontab (run once daily at 2am)
0 2 * * * cd /path/to/traefik-n8n && ./backup.sh
```

### View Logs

```bash
# View Traefik logs
docker-compose logs -f traefik

# View n8n logs
docker-compose logs -f n8n

# View all logs
docker-compose logs -f
```

## 🚨 Troubleshooting

| Symptom | Solution |
|---------|----------|
| Certificate errors | Run `rm traefik/acme/cloudflare.json` and restart |
| n8n not accessible | Check `docker-compose ps` for running containers |
| 404 errors | Verify Traefik labels and network configuration |
| Database issues | Check PostgreSQL logs with `docker-compose logs postgres` |
| Connection refused | Verify ports 80 and 443 are open on your firewall |
| n8n crashes | Check for sufficient memory (min 2GB recommended) |

### Common Commands

```bash
# Check container status
docker-compose ps

# Check n8n logs
docker-compose logs -f n8n

# Restart all services
docker-compose restart

# Stop all services
docker-compose down

# Start all services
docker-compose up -d
```

## 🙋 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgements

- [n8n](https://n8n.io) - Workflow automation tool
- [Traefik](https://traefik.io) - Modern HTTP reverse proxy
- [Docker](https://www.docker.com) - Container platform
- [Let's Encrypt](https://letsencrypt.org) - Free, automated certificates
