# HackGPT Enterprise - Startup Guide

This guide provides comprehensive instructions for starting HackGPT Enterprise in various deployment modes.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Quick Start](#quick-start)
3. [Installation Setup](#installation-setup)
4. [Deployment Modes](#deployment-modes)
5. [Configuration](#configuration)
6. [Verification](#verification)
7. [Troubleshooting](#troubleshooting)
8. [Service Management](#service-management)

---

## Prerequisites

### System Requirements

- **Operating System**: Linux (Ubuntu 20.04+, Debian 10+, RHEL 8+, CentOS 8+), macOS 11+, or Windows WSL2
- **Python**: 3.8+ with pip and virtual environment support
- **Docker**: 20.10+ (for containerized deployment)
- **Docker Compose**: 1.29+ (for full stack deployment)
- **Memory**: Minimum 4GB RAM (8GB+ recommended for full stack)
- **Disk Space**: Minimum 20GB free space
- **Network**: Ports 8000, 8080, 5432, 6379, 9090, 3000, 5601 available (varies by mode)

### Required Tools (for full stack)

- `git` - Version control
- `python3` - Python interpreter
- `pip` - Python package manager
- `docker` - Container runtime
- `docker-compose` - Container orchestration
- `curl` or `wget` - For health checks

### Supported Python Versions

- Python 3.8 (minimum)
- Python 3.9 (recommended)
- Python 3.10 (tested)
- Python 3.11 (tested)
- Python 3.12+ (compatible)

---

## Quick Start

### 1-Minute Quick Start

```bash
# Clone repository
git clone https://github.com/yashab-cyber/HackGPT.git
cd HackGPT

# Create Python environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env

# Start application
python3 advance_hackgpt.py
```

---

## Installation Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/yashab-cyber/HackGPT.git
cd HackGPT
```

### Step 2: Run the Installation Script

The `install.sh` script automates dependency installation:

```bash
# Make script executable
chmod +x install.sh

# Run installation (Debian/Ubuntu)
./install.sh
```

**What install.sh does:**
- Installs system dependencies (nmap, sqlmap, hydra, etc.)
- Creates Python virtual environment
- Installs Python dependencies from requirements.txt
- Installs Ollama (optional local LLM)
- Sets up database connections
- Symlinks to /usr/local/bin/hackgpt (v1)

### Step 3: Manual Setup (Alternative)

If `install.sh` doesn't work on your system:

```bash
# Update package manager
sudo apt-get update && sudo apt-get upgrade -y  # Debian/Ubuntu

# Install system dependencies
sudo apt-get install -y \
    python3.9 \
    python3-pip \
    python3-venv \
    git \
    curl \
    wget \
    docker.io \
    docker-compose \
    postgresql \
    redis-server

# Create Python virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # Linux/macOS
# or
venv\Scripts\activate  # Windows

# Install Python dependencies
pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
pip install -r requirements_enterprise.txt  # For enterprise features
pip install -r requirements-dev.txt  # For development
```

### Step 4: Configure Environment

```bash
# Copy environment template
cp .env.example .env

# Edit configuration (use your preferred editor)
nano .env
# or
vi .env

# Key variables to configure:
# - OPENAI_API_KEY (if using OpenAI)
# - ANTHROPIC_API_KEY (if using Claude)
# - DATABASE_URL (PostgreSQL connection)
# - REDIS_URL (Redis connection)
# - AUTH_KEY (enterprise authentication)
```

### Step 5: Verify Installation

```bash
# Test installation
python3 test_installation.py

# Expected output:
# ✓ HackGPT installation verified
# ✓ All dependencies installed
# ✓ Required tools available
# ✓ Database connectivity OK
# ✓ Redis connectivity OK
```

---

## Deployment Modes

HackGPT supports 5 deployment modes. Choose based on your needs:

### Mode 1: Interactive Enterprise Mode (Default)

**Best for**: Manual pentesting, learning, development

```bash
# Activate virtual environment
source venv/bin/activate

# Start interactive mode
python3 advance_hackgpt.py

# Expected output:
# ╔════════════════════════════════════════╗
# ║     HackGPT Enterprise v2026.09.19     ║
# ╚════════════════════════════════════════╝
# 
# Main Menu:
# 1. New Penetration Test
# 2. View Results
# 3. Configuration
# 4. Exit
#
# Select option: 
```

**Features:**
- Interactive menu system
- Real-time prompts
- Manual workflow control
- Voice interface (if configured)

**Stop:** Press `Ctrl+C` to exit

---

### Mode 2: API Server Mode

**Best for**: Integration, automation, CI/CD pipelines

```bash
# Activate virtual environment
source venv/bin/activate

# Start API server
python3 advance_hackgpt.py --api

# Expected output:
# [INFO] Starting HackGPT API Server v2026.09.19
# [INFO] Listening on 0.0.0.0:8000
# [INFO] API documentation: http://localhost:8000/docs
# [INFO] Health check: http://localhost:8000/api/health
```

**Access:**
- API: `http://localhost:8000`
- API Docs (Swagger): `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`
- Health Check: `http://localhost:8000/api/health`

**Test API:**
```bash
# Health check
curl http://localhost:8000/api/health

# List assessments
curl http://localhost:8000/api/assessments \
  -H "Authorization: ******"
```

**Stop:** Press `Ctrl+C` to stop the server

---

### Mode 3: Web Dashboard Mode

**Best for**: Visual workflow, monitoring, team collaboration

```bash
# Activate virtual environment
source venv/bin/activate

# Start web dashboard
python3 advance_hackgpt.py --web

# Expected output:
# [INFO] Starting HackGPT Web Dashboard
# [INFO] Dashboard listening on 0.0.0.0:8080
# [INFO] Open browser: http://localhost:8080
```

**Access:**
- Web UI: `http://localhost:8080`
- Login with enterprise credentials
- Interactive assessment workflow
- Real-time dashboards

**Stop:** Press `Ctrl+C` or use web interface logout

---

### Mode 4: Full Enterprise Stack (Recommended for Production)

**Best for**: Production deployments, multi-user environments, scaling

```bash
# Ensure .env is configured
cat .env

# Start full stack
docker-compose up -d

# Monitor startup (30-60 seconds)
docker-compose logs -f

# Expected output:
# hackgpt-app        | [INFO] HackGPT API ready
# hackgpt-worker     | [INFO] Celery worker started
# postgres           | ready to accept connections
# redis              | Ready to accept connections
```

**Services started:**
- **HackGPT App**: http://localhost:8000 (API) & http://localhost:8080 (Web)
- **PostgreSQL**: localhost:5432 (database)
- **Redis**: localhost:6379 (cache/queue)
- **Celery Worker**: Background task processing
- **Celery Beat**: Job scheduling
- **Flower**: http://localhost:5555 (Celery monitoring)
- **Prometheus**: http://localhost:9090 (metrics)
- **Grafana**: http://localhost:3000 (dashboards)
- **Elasticsearch**: localhost:9200 (logging)
- **Kibana**: http://localhost:5601 (log analytics)
- **Consul**: http://localhost:8500 (service registry)
- **Nginx**: http://localhost:80 (reverse proxy)

**Verify stack:**
```bash
# Check all containers running
docker-compose ps

# Check specific service
docker-compose logs hackgpt-app
docker-compose logs postgres

# Check service health
curl http://localhost:8000/api/health
curl http://localhost:3000  # Grafana
curl http://localhost:5601  # Kibana
```

**Stop stack:**
```bash
# Stop services
docker-compose down

# Stop and remove volumes (WARNING: deletes data!)
docker-compose down -v
```

---

### Mode 5: Direct Assessment Mode

**Best for**: Automated scanning, CI/CD integration, headless operation

```bash
# Activate virtual environment
source venv/bin/activate

# Run direct assessment
python3 advance_hackgpt.py \
  --target example.com \
  --scope "Web application and API" \
  --auth-key "YOUR_ENTERPRISE_AUTH_KEY" \
  --assessment-type black-box \
  --compliance OWASP \
  --ai-model gpt-4-turbo \
  --output-format json

# All-in-one example (all phases)
python3 advance_hackgpt.py \
  --target 192.168.1.0/24 \
  --scope "Internal Network Assessment" \
  --auth-key "ENTERPRISE-AUTH-2025" \
  --assessment-type white-box \
  --compliance "NIST,ISO27001" \
  --ai-provider anthropic \
  --ai-model claude-opus-4 \
  --output-format html,pdf,json \
  --report-dir ./reports \
  --parallel-workers 4 \
  --timeout 3600
```

**Output:**
```
[INFO] Assessment started for example.com
[PHASE 1] Reconnaissance...
[PHASE 2] Scanning...
[PHASE 3] Enumeration...
[PHASE 4] Exploitation...
[PHASE 5] Analysis...
[PHASE 6] Reporting...
[SUCCESS] Report: ./reports/example.com_2025-01-15.html
```

---

## Configuration

### Environment Variables (.env)

Required variables to set before startup:

```bash
# API Keys (at least one AI provider required)
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GOOGLE_API_KEY=...
DEEPSEEK_API_KEY=...
GLM_API_KEY=...
OPENROUTER_API_KEY=...

# Database Configuration
DATABASE_URL=******localhost:5432/hackgpt
DATABASE_POOL_SIZE=20
DATABASE_MAX_OVERFLOW=40

# Redis Configuration
REDIS_URL=redis://localhost:6379/0
REDIS_PASSWORD=your_redis_password

# Authentication
AUTH_KEY=your_enterprise_auth_key_here
JWT_SECRET=your_jwt_secret_here
LDAP_SERVER=ldap://your-ad-server.com

# Application Settings
APP_ENV=production  # or development
DEBUG=false
LOG_LEVEL=INFO
WORKERS=4

# Port Configuration
API_PORT=8000
WEB_PORT=8080

# Email Configuration (for reports)
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=your-app-password

# AWS Configuration (if using AWS)
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
AWS_REGION=us-east-1

# Ollama Configuration (for local LLM)
OLLAMA_HOST=http://localhost:11434
OLLAMA_MODEL=mistral
```

### Configuration File (config.ini)

Alternative configuration method:

```ini
[database]
url = ******localhost:5432/hackgpt
pool_size = 20

[redis]
url = redis://localhost:6379/0
password = your_password

[ai_engine]
default_provider = openai
default_model = gpt-4-turbo
fallback_provider = anthropic

[security]
auth_key = your_auth_key
jwt_expiry = 3600

[logging]
level = INFO
format = json
file = logs/hackgpt.log
```

### Port Configuration

Adjust ports if defaults are in use:

```bash
# API Server (default 8000)
python3 advance_hackgpt.py --api --port 8001

# Web Dashboard (default 8080)
python3 advance_hackgpt.py --web --port 8081

# docker-compose
export API_PORT=8001
export WEB_PORT=8081
docker-compose up -d
```

---

## Verification

### Health Checks

After startup, verify everything works:

```bash
# API Health
curl -i http://localhost:8000/api/health
# Expected: HTTP 200 OK

# Database Connection
curl http://localhost:8000/api/health/database
# Expected: "database": "connected"

# Redis Connection
curl http://localhost:8000/api/health/redis
# Expected: "redis": "connected"

# AI Engine Status
curl http://localhost:8000/api/health/ai
# Expected: "ai_providers": ["openai", "anthropic", ...]
```

### Web Dashboard Access

```bash
# Open in browser
http://localhost:8080

# Login with credentials:
# Default admin: admin / admin
# (Change in production!)

# Should show:
# - Dashboard overview
# - Recent assessments
# - System health
# - AI model status
```

### API Documentation

```bash
# Access Swagger UI
http://localhost:8000/docs

# Test API endpoints:
# - GET /api/assessments
# - POST /api/assessments/create
# - GET /api/assessments/{id}
# - GET /api/reports/{id}
```

### Docker Stack Verification

```bash
# Check all services running
docker-compose ps
# All containers should show "Up" status

# Check logs
docker-compose logs -f hackgpt-app

# Enter container shell
docker exec -it hackgpt-app bash

# Database check
docker exec hackgpt-postgres psql -U hackgpt -c "SELECT version();"

# Redis check
docker exec hackgpt-redis redis-cli ping
# Expected: PONG
```

---

## Troubleshooting

### Common Issues and Solutions

#### 1. "Python not found" or "Python 3.8+ required"

```bash
# Check Python version
python3 --version

# Install Python 3.9+
# Ubuntu/Debian
sudo apt-get install python3.9 python3.9-venv

# macOS
brew install python@3.9

# Windows: Download from python.org
```

#### 2. "ModuleNotFoundError: No module named 'X'"

```bash
# Activate virtual environment
source venv/bin/activate

# Reinstall dependencies
pip install --upgrade pip
pip install -r requirements.txt
pip install -r requirements_enterprise.txt

# Verify installation
python3 -c "import advance_hackgpt; print('OK')"
```

#### 3. "Address already in use" (Port conflicts)

```bash
# Check what's using port 8000
lsof -i :8000
# or
netstat -tulpn | grep 8000

# Kill process on port 8000
kill -9 <PID>

# Or use different port
python3 advance_hackgpt.py --api --port 8001
```

#### 4. Database Connection Error

```bash
# Verify PostgreSQL is running
sudo systemctl status postgresql
# or
docker-compose logs postgres

# Test database connection
psql -h localhost -U hackgpt -c "SELECT 1;"

# If docker: Check credentials in .env
docker-compose logs postgres | grep -i error

# Recreate database
docker-compose down
rm -rf postgres_data/
docker-compose up -d postgres
sleep 10
docker-compose up -d
```

#### 5. Redis Connection Error

```bash
# Check Redis status
redis-cli ping
# Expected: PONG

# Or in docker
docker-compose logs redis
docker exec hackgpt-redis redis-cli ping

# Restart Redis
docker-compose restart redis
```

#### 6. API Server Won't Start

```bash
# Check error details
python3 advance_hackgpt.py --api --debug

# Verify configuration
python3 -c "from advance_hackgpt import Config; c = Config(); print(c)"

# Check dependencies
python3 test_installation.py

# View logs
tail -f logs/hackgpt.log
```

#### 7. Web Dashboard Not Accessible

```bash
# Check if process is running
ps aux | grep advance_hackgpt

# Check logs
docker-compose logs hackgpt-app

# Try with explicit host
python3 advance_hackgpt.py --web --host 0.0.0.0

# Check firewall
sudo ufw status
sudo ufw allow 8080  # If needed
```

#### 8. Celery Workers Not Processing Tasks

```bash
# Check worker status
docker-compose logs celery-worker

# Restart workers
docker-compose restart celery-worker celery-beat

# Check task queue
docker exec hackgpt-redis redis-cli LLEN celery

# Monitor with Flower
open http://localhost:5555
```

#### 9. Out of Memory Errors

```bash
# Increase available memory
docker-compose down
# Edit docker-compose.yml: set mem_limit

# Or check system memory
free -h  # Linux
vm_stat  # macOS
```

#### 10. Slow Startup / Hanging

```bash
# Run with verbose logging
DEBUG=true python3 advance_hackgpt.py --api --debug

# Check system resources
top -b -n 1 | head -20

# Check disk space
df -h

# Increase timeout in docker-compose
# Set healthcheck interval higher
```

### Debug Mode

```bash
# Run with debug logging
export DEBUG=true
export LOG_LEVEL=DEBUG
python3 advance_hackgpt.py --api

# Or via environment
DEBUG=true python3 advance_hackgpt.py --api --debug

# Check detailed logs
tail -f logs/hackgpt.log
cat logs/error.log
```

### Support Resources

- **Documentation**: See `/doc/` directory
- **Issues**: Report on GitHub Issues
- **Discord**: Join community server for live support
- **Email**: support@hackgpt.io

---

## Service Management

### Starting Services

```bash
# Single mode
python3 advance_hackgpt.py --api

# Multiple modes with systemd
# Create /etc/systemd/system/hackgpt-api.service
[Unit]
Description=HackGPT API Server
After=network.target

[Service]
Type=simple
User=hackgpt
WorkingDirectory=/opt/hackgpt
Environment="PATH=/opt/hackgpt/venv/bin"
ExecStart=/opt/hackgpt/venv/bin/python3 /opt/hackgpt/advance_hackgpt.py --api
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target

# Enable and start
sudo systemctl daemon-reload
sudo systemctl enable hackgpt-api
sudo systemctl start hackgpt-api
sudo systemctl status hackgpt-api
```

### Stopping Services

```bash
# Single service
Ctrl+C

# Docker stack
docker-compose down

# Systemd
sudo systemctl stop hackgpt-api

# Kill all HackGPT processes
pkill -f advance_hackgpt
```

### Restarting Services

```bash
# Docker stack
docker-compose restart

# Single service
docker-compose restart hackgpt-app

# Systemd
sudo systemctl restart hackgpt-api
```

### Viewing Logs

```bash
# Real-time logs
docker-compose logs -f

# Specific service
docker-compose logs -f hackgpt-app

# Historical logs
docker-compose logs --tail 100 hackgpt-app

# Application logs
tail -f logs/hackgpt.log
tail -f logs/error.log
```

---

## Quick Reference

### Commands Summary

```bash
# Installation
git clone https://github.com/yashab-cyber/HackGPT.git
cd HackGPT
chmod +x install.sh && ./install.sh
cp .env.example .env
python3 test_installation.py

# Start Modes
source venv/bin/activate

# Interactive
python3 advance_hackgpt.py

# API Server
python3 advance_hackgpt.py --api

# Web Dashboard
python3 advance_hackgpt.py --web

# Full Stack
docker-compose up -d

# Monitoring
docker-compose logs -f
curl http://localhost:8000/api/health
open http://localhost:8080

# Stop
Ctrl+C
docker-compose down
```

### Default Ports

| Service | Port | URL |
|---------|------|-----|
| API Server | 8000 | http://localhost:8000 |
| Web Dashboard | 8080 | http://localhost:8080 |
| PostgreSQL | 5432 | localhost:5432 |
| Redis | 6379 | localhost:6379 |
| Prometheus | 9090 | http://localhost:9090 |
| Grafana | 3000 | http://localhost:3000 |
| Kibana | 5601 | http://localhost:5601 |
| Flower (Celery) | 5555 | http://localhost:5555 |
| Consul | 8500 | http://localhost:8500 |

### API Endpoints (Quick Reference)

```bash
# Health & Status
GET  /api/health
GET  /api/status
GET  /api/health/database
GET  /api/health/redis

# Assessments
GET  /api/assessments
POST /api/assessments/create
GET  /api/assessments/{id}
PUT  /api/assessments/{id}
DELETE /api/assessments/{id}

# Reports
GET  /api/reports
GET  /api/reports/{id}
POST /api/reports/{id}/download

# Authentication
POST /api/auth/login
POST /api/auth/logout
POST /api/auth/refresh

# Documentation
GET  /docs (Swagger UI)
GET  /redoc (ReDoc)
```

---

## Next Steps

1. **Review Configuration**: Ensure `.env` has all required API keys
2. **Run Tests**: Execute `python3 test_installation.py`
3. **Choose Mode**: Select appropriate deployment mode for your use case
4. **Start Service**: Follow instructions for your chosen mode
5. **Verify Health**: Run health checks to ensure everything works
6. **Configure Monitoring**: Set up Grafana dashboards in production
7. **Enable Logging**: Configure ELK stack for detailed logs
8. **Set Up Backup**: Plan database and report backup strategy

---

**Version**: 2026.09.19  
**Last Updated**: 2025-01-15  
**Status**: Production Ready
