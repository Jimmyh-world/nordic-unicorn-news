# Nordic Unicorn News - Production Deployment

**Deployed:** 2025-10-20
**Environment:** Beast (Home Lab)
**Status:** ✅ LIVE
**URL:** https://nordicunicornnews.com

---

## Deployment Summary

Nordic Unicorn News "Coming Soon" page is deployed on Beast using Docker + Cloudflare Tunnel.

### Infrastructure

**Container:**
- Name: `nordic-unicorn-news`
- Image: `nginx:alpine` (custom build)
- Port: 3001 (host) → 80 (container)
- Status: Healthy, auto-restart enabled

**Cloudflare Tunnel:**
- Tunnel ID: `c0286bf5-701f-406f-bda5-c9bc2d196bd2`
- Tunnel Name: `nordic-unicorn-news`
- Config: `~/nordic-unicorn-news/cloudflare/config.yml`
- Process: Background process (PID varies)
- Connections: 4 active edge connections

**Domains:**
- https://nordicunicornnews.com → localhost:3001
- https://www.nordicunicornnews.com → localhost:3001

---

## Quick Reference Commands

### Check Status

```bash
# Container status
docker ps --filter "name=nordic-unicorn-news"

# Container logs
docker logs nordic-unicorn-news --tail 50

# Tunnel status
cloudflared tunnel info nordic-unicorn-news

# Tunnel process
ps aux | grep "cloudflared.*nordic-unicorn-news" | grep -v grep

# Tunnel logs
tail -f ~/nordic-unicorn-news/cloudflare/tunnel.log
```

### Test Endpoints

```bash
# Local container
curl http://localhost:3001
curl http://localhost:3001/health

# Live site (requires external DNS)
curl https://nordicunicornnews.com
curl https://www.nordicunicornnews.com
```

### Restart Services

```bash
# Restart container
cd ~/nordic-unicorn-news
docker compose restart

# Restart tunnel
pkill -f "cloudflared.*nordic-unicorn-news"
sleep 2
nohup cloudflared --config ~/nordic-unicorn-news/cloudflare/config.yml tunnel run nordic-unicorn-news > ~/nordic-unicorn-news/cloudflare/tunnel.log 2>&1 &

# Restart both
docker compose restart && pkill -f "cloudflared.*nordic-unicorn-news" && sleep 2 && nohup cloudflared --config ~/nordic-unicorn-news/cloudflare/config.yml tunnel run nordic-unicorn-news > ~/nordic-unicorn-news/cloudflare/tunnel.log 2>&1 &
```

### Stop Services

```bash
# Stop container
cd ~/nordic-unicorn-news
docker compose down

# Stop tunnel
pkill -f "cloudflared.*nordic-unicorn-news"

# Stop both
docker compose down && pkill -f "cloudflared.*nordic-unicorn-news"
```

### Update Deployment

```bash
# Pull latest changes
cd ~/nordic-unicorn-news
git pull origin main

# Rebuild and restart container
docker compose up -d --build

# Tunnel will automatically route to new container
```

---

## Deployment Architecture

```
Internet
    ↓
Cloudflare Edge (arn02, arn06, arn07)
    ↓
Cloudflare Tunnel (c0286bf5-701f-406f-bda5-c9bc2d196bd2)
    ↓
Beast Host Process (cloudflared)
    ↓
localhost:3001
    ↓
Docker Container (nordic-unicorn-news)
    ↓
nginx:alpine serving static HTML
```

---

## Files & Directories

```
~/nordic-unicorn-news/
├── public/                      # Static site content
│   ├── index.html              # Coming Soon page
│   └── assets/                 # Images, SVG logo
├── docker/
│   └── nginx.conf              # Nginx configuration
├── cloudflare/
│   ├── config.yml              # Tunnel configuration
│   └── tunnel.log              # Tunnel runtime logs
├── docker-compose.yml          # Container orchestration
├── Dockerfile                  # Container build instructions
└── docs/
    └── DEPLOYMENT.md           # This file
```

**Credentials:**
- `~/.cloudflared/c0286bf5-701f-406f-bda5-c9bc2d196bd2.json` (tunnel credentials)
- `~/.cloudflared/cert.pem` (Cloudflare origin certificate)

---

## Health Checks

### Container Health

Docker healthcheck runs every 30 seconds:
```bash
wget --quiet --tries=1 --spider http://127.0.0.1:80/health
```

Expected: HTTP 200 with response "healthy"

Check health status:
```bash
docker inspect nordic-unicorn-news --format='{{.State.Health.Status}}'
# Expected: healthy
```

### Tunnel Health

Tunnel maintains 4 active connections to Cloudflare edge:
```bash
cloudflared tunnel info nordic-unicorn-news
```

Expected output includes:
```
CONNECTOR ID                         CREATED              ARCHITECTURE VERSION   ORIGIN IP   EDGE
[uuid]                              [timestamp]          linux_amd64  2025.10.0 [IP]       2xarn02, 1xarn06, 1xarn07
```

---

## Troubleshooting

### Container Not Responding

**Symptom:** `curl http://localhost:3001` fails

**Solution:**
```bash
# Check container status
docker ps --filter "name=nordic-unicorn-news"

# Check logs
docker logs nordic-unicorn-news

# Restart if needed
cd ~/nordic-unicorn-news
docker compose restart

# Rebuild if needed
docker compose up -d --build
```

### Site Returns 502 Bad Gateway

**Symptom:** https://nordicunicornnews.com returns 502

**Solutions:**

1. Verify container is running:
```bash
docker ps --filter "name=nordic-unicorn-news"
curl http://localhost:3001
```

2. Verify tunnel is running:
```bash
ps aux | grep "cloudflared.*nordic-unicorn-news" | grep -v grep
cloudflared tunnel info nordic-unicorn-news
```

3. Check tunnel can reach container:
```bash
tail -50 ~/nordic-unicorn-news/cloudflare/tunnel.log
# Look for connection errors
```

4. Restart tunnel:
```bash
pkill -f "cloudflared.*nordic-unicorn-news"
sleep 2
cd ~/nordic-unicorn-news
nohup cloudflared --config cloudflare/config.yml tunnel run nordic-unicorn-news > cloudflare/tunnel.log 2>&1 &
```

### Tunnel Not Connecting

**Symptom:** `cloudflared tunnel info` shows no connections

**Solutions:**

1. Check credentials exist:
```bash
ls -la ~/.cloudflared/c0286bf5-701f-406f-bda5-c9bc2d196bd2.json
```

2. Check tunnel log:
```bash
tail -50 ~/nordic-unicorn-news/cloudflare/tunnel.log | grep ERR
```

3. Restart tunnel with fresh log:
```bash
pkill -f "cloudflared.*nordic-unicorn-news"
rm -f ~/nordic-unicorn-news/cloudflare/tunnel.log
sleep 2
cd ~/nordic-unicorn-news
nohup cloudflared --config cloudflare/config.yml tunnel run nordic-unicorn-news > cloudflare/tunnel.log 2>&1 &
sleep 5
tail -30 cloudflare/tunnel.log
```

### Container Marked Unhealthy

**Symptom:** `docker ps` shows container as "unhealthy"

**Solution:**

1. Test health endpoint manually:
```bash
curl http://localhost:3001/health
# Expected: "healthy"
```

2. Check Docker health logs:
```bash
docker inspect nordic-unicorn-news --format='{{json .State.Health}}' | python3 -m json.tool
```

3. If health endpoint works manually but Docker reports unhealthy, restart:
```bash
cd ~/nordic-unicorn-news
docker compose restart
```

---

## Security

### TLS/HTTPS

- All external traffic encrypted via Cloudflare Tunnel
- Cloudflare provides TLS termination
- Local container uses HTTP (localhost only)

### Firewall

- Port 3001 only accessible from localhost
- Cloudflare tunnel uses outbound HTTPS (443)
- No inbound ports exposed to internet

### Credentials

Tunnel credentials stored at:
- `~/.cloudflared/c0286bf5-701f-406f-bda5-c9bc2d196bd2.json` (mode 400)
- Never commit to git (in .gitignore)

---

## Performance

### Response Times

From external monitoring (Cloudflare):
- Typical: 50-150ms (including tunnel + nginx)
- Geographic: Edge routing via arn02/arn06/arn07 (Stockholm region)

### Resource Usage

**Container:**
- CPU: <1%
- Memory: ~10MB
- Disk: 150MB (nginx:alpine image)

**Tunnel Process:**
- CPU: <1%
- Memory: ~40MB
- Network: Minimal (static site, low traffic)

---

## Monitoring

### Access Logs

Container access logs:
```bash
docker logs nordic-unicorn-news
```

Shows:
- IP addresses (via Cloudflare tunnel)
- Request paths
- Response codes
- User agents

### Metrics

Available via Beast monitoring stack:

**Prometheus:** http://localhost:9090
- Container metrics via cAdvisor
- Host metrics via node-exporter

**Grafana:** https://grafana.kitt.agency
- Pre-built dashboards
- Container performance
- System resources

---

## Maintenance

### Regular Tasks

**Daily:**
- No action required (auto-restart enabled)

**Weekly:**
- Check tunnel connections: `cloudflared tunnel info nordic-unicorn-news`
- Review container logs: `docker logs nordic-unicorn-news --tail 100`

**Monthly:**
- Update base image: `docker compose pull && docker compose up -d`
- Review Cloudflare analytics

### Updates

**Content Updates:**
```bash
cd ~/nordic-unicorn-news
# Edit files in public/
docker compose restart
```

**Configuration Updates:**
```bash
cd ~/nordic-unicorn-news
# Edit docker-compose.yml or cloudflare/config.yml
docker compose up -d  # For container config
# For tunnel config, restart tunnel process
```

**System Updates:**
```bash
# Update cloudflared
sudo cloudflared update

# Update Docker images
cd ~/nordic-unicorn-news
docker compose pull
docker compose up -d
```

---

## Rollback Procedure

If deployment fails:

```bash
# Stop services
cd ~/nordic-unicorn-news
docker compose down
pkill -f "cloudflared.*nordic-unicorn-news"

# Revert code
git log --oneline -5
git checkout [previous-commit-hash]

# Rebuild
docker compose up -d --build

# Restart tunnel
nohup cloudflared --config cloudflare/config.yml tunnel run nordic-unicorn-news > cloudflare/tunnel.log 2>&1 &

# Verify
curl http://localhost:3001
cloudflared tunnel info nordic-unicorn-news
```

---

## Deployment Checklist

When deploying updates:

- [ ] Pull latest code: `git pull origin main`
- [ ] Review changes: `git log --oneline -5`
- [ ] Rebuild container: `docker compose up -d --build`
- [ ] Verify container health: `docker ps`
- [ ] Test local endpoint: `curl http://localhost:3001`
- [ ] Test health endpoint: `curl http://localhost:3001/health`
- [ ] Verify tunnel running: `cloudflared tunnel info nordic-unicorn-news`
- [ ] Test live site: `curl https://nordicunicornnews.com`
- [ ] Check logs for errors: `docker logs nordic-unicorn-news --tail 50`
- [ ] Verify in browser: https://nordicunicornnews.com

---

## Support

**Issues:** https://github.com/Jimmyh-world/nordic-unicorn-news/issues

**Documentation:**
- Cloudflare Tunnels: https://developers.cloudflare.com/cloudflare-one/connections/connect-applications/
- Docker Compose: https://docs.docker.com/compose/
- nginx: https://nginx.org/en/docs/

---

**Last Updated:** 2025-10-20
**Deployed By:** Beast Executor
**Deployment Method:** Jimmy's Workflow (RED→GREEN→CHECKPOINT)
