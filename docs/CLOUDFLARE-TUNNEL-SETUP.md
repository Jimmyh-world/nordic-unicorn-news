# Cloudflare Tunnel Setup for nordicunicornnews.com

**Domain:** nordicunicornnews.com
**Purchased:** 2025-10-20 via Cloudflare
**Target:** Beast (192.168.68.100)
**Application:** Nordic Unicorn News (Svelte + Express)

---

## Overview

This guide covers setting up Cloudflare Tunnel to expose the Nordic Unicorn News application running on Beast to the public internet via nordicunicornnews.com.

**Reference:** Beast already has successful tunnel deployments:
- `kitt.agency` (monitoring stack)
- `web3studio.dev` (Mundus staging)

See `~/dev-network/beast/cloudflare/` for existing configs.

---

## Prerequisites

✅ Domain purchased via Cloudflare: nordicunicornnews.com
✅ Beast available at 192.168.68.100
✅ Cloudflared installed on Beast
✅ Application will run on port (TBD - likely 3000 or 8080)

---

## Setup Steps (To Be Executed on Beast)

### Step 1: Create Tunnel

```bash
# SSH to Beast
ssh jimmyb@192.168.68.100

# Create tunnel for Nordic Unicorn News
cloudflared tunnel create nordic-unicorn-news

# Note the tunnel ID from output
```

**Output Example:**
```
Created tunnel nordic-unicorn-news with id: <TUNNEL_ID>
```

---

### Step 2: Configure DNS

```bash
# Route nordicunicornnews.com to the tunnel
cloudflared tunnel route dns nordic-unicorn-news nordicunicornnews.com

# Optionally route www subdomain
cloudflared tunnel route dns nordic-unicorn-news www.nordicunicornnews.com
```

**Verification:**
Check Cloudflare dashboard → nordicunicornnews.com → DNS records
Should see CNAME records pointing to `<TUNNEL_ID>.cfargotunnel.com`

---

### Step 3: Create Tunnel Configuration

Create config file on Beast:

```bash
# Create directory structure
mkdir -p ~/dev-network/beast/cloudflare

# Create config file
nano ~/dev-network/beast/cloudflare/config-nordicunicorn.yml
```

**Config Template:**
```yaml
tunnel: <TUNNEL_ID>
credentials-file: /home/jimmyb/.cloudflared/<TUNNEL_ID>.json

ingress:
  # Main site
  - hostname: nordicunicornnews.com
    service: http://localhost:3000
    originRequest:
      noTLSVerify: true

  # WWW redirect (optional)
  - hostname: www.nordicunicornnews.com
    service: http://localhost:3000
    originRequest:
      noTLSVerify: true

  # Catch-all (required by Cloudflare)
  - service: http_status:404
```

**Notes:**
- Replace `<TUNNEL_ID>` with actual tunnel ID
- Adjust port (3000) based on actual application port
- Consider adding subdomains (api.nordicunicornnews.com, admin.nordicunicornnews.com)

---

### Step 4: Test Tunnel

```bash
# Test tunnel configuration
cloudflared tunnel --config ~/dev-network/beast/cloudflare/config-nordicunicorn.yml run

# Watch logs for connection
# Should see: "Connection <X> registered"
```

**Verification:**
- Visit https://nordicunicornnews.com
- Should connect (will show error until app is running)
- Check tunnel status: `cloudflared tunnel info nordic-unicorn-news`

---

### Step 5: Run Tunnel Persistently

**Option A: Background Process (Quick Test)**
```bash
cd ~/dev-network/beast
nohup cloudflared tunnel --config cloudflare/config-nordicunicorn.yml run > /tmp/cloudflared-unicorn.log 2>&1 &
```

**Option B: Systemd Service (Production)**
```bash
# Create systemd service file
sudo nano /etc/systemd/system/cloudflared-nordicunicorn.service
```

**Service Template:**
```ini
[Unit]
Description=Cloudflare Tunnel - Nordic Unicorn News
After=network.target

[Service]
Type=simple
User=jimmyb
ExecStart=/usr/local/bin/cloudflared tunnel --config /home/jimmyb/dev-network/beast/cloudflare/config-nordicunicorn.yml run
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

**Enable and start:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable cloudflared-nordicunicorn
sudo systemctl start cloudflared-nordicunicorn
sudo systemctl status cloudflared-nordicunicorn
```

---

## Application Deployment

Once tunnel is configured, the application needs to:

1. **Run on correct port** (configured in tunnel ingress)
2. **Accept connections from localhost** (Cloudflare Tunnel forwards to localhost)
3. **Handle HTTPS properly** (Cloudflare handles SSL termination)

**Docker Compose Example:**
```yaml
services:
  nordic-unicorn-news:
    build: .
    ports:
      - "3000:3000"  # Match tunnel config
    environment:
      - NODE_ENV=production
      - PORT=3000
```

---

## DNS Configuration (Cloudflare Dashboard)

**After tunnel setup, verify in Cloudflare:**

1. Go to nordicunicornnews.com → DNS
2. Should see CNAME records:
   - `nordicunicornnews.com` → `<TUNNEL_ID>.cfargotunnel.com`
   - `www.nordicunicornnews.com` → `<TUNNEL_ID>.cfargotunnel.com` (if configured)

3. Optional: Add more subdomains
   - `api.nordicunicornnews.com` - API endpoint
   - `admin.nordicunicornnews.com` - Admin panel

---

## Validation Checklist

After setup:

- [ ] Tunnel created: `cloudflared tunnel list`
- [ ] DNS configured: Check Cloudflare dashboard
- [ ] Config file created: `~/dev-network/beast/cloudflare/config-nordicunicorn.yml`
- [ ] Tunnel running: `cloudflared tunnel info nordic-unicorn-news`
- [ ] HTTPS accessible: `curl -I https://nordicunicornnews.com`
- [ ] Service running: `systemctl status cloudflared-nordicunicorn` (if systemd)

---

## Troubleshooting

**Issue:** DNS not resolving
**Solution:** Wait 5-10 minutes for DNS propagation. Check `dig nordicunicornnews.com`

**Issue:** Tunnel shows "Connection registered" but site unreachable
**Solution:** Check application is running on configured port: `netstat -tlnp | grep 3000`

**Issue:** SSL/TLS errors
**Solution:** Verify Cloudflare SSL mode is "Full" (not "Flexible" or "Strict")

**Issue:** 502 Bad Gateway
**Solution:** Application not running or wrong port in tunnel config

---

## Security Considerations

**Cloudflare Settings:**
- Enable "Always Use HTTPS"
- Enable "HSTS" (HTTP Strict Transport Security)
- Set SSL/TLS mode to "Full"
- Enable "WAF" (Web Application Firewall)
- Consider "Rate Limiting" rules

**Application:**
- Trust Cloudflare proxy headers (`CF-Connecting-IP`, `X-Forwarded-For`)
- Validate origin is from Cloudflare (IP ranges)
- Implement proper CORS if needed

---

## Monitoring

**Tunnel Health:**
```bash
# Check tunnel status
cloudflared tunnel info nordic-unicorn-news

# View logs
journalctl -u cloudflared-nordicunicorn -f
# OR (if background)
tail -f /tmp/cloudflared-unicorn.log
```

**Application Health:**
- Integrate with Prometheus/Grafana (already deployed on Beast)
- Monitor uptime, response times, error rates

---

## Cost

**Cloudflare Tunnel:** FREE (included with Cloudflare account)
**Domain:** ~$10-15/year (nordicunicornnews.com)

**Total Infrastructure Cost:** $0/month (using home lab) + domain registration

---

## References

- **Existing Configs:** `~/dev-network/beast/cloudflare/`
  - `config.yml` (kitt.agency)
  - `config-web3studio.yml` (Mundus staging)
- **Cloudflare Tunnel Docs:** https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/
- **Beast Infrastructure:** `~/dev-network/README.md`

---

**Created:** 2025-10-20
**Status:** Documentation complete, tunnel not yet configured
**Next Step:** Execute Steps 1-5 on Beast when application is ready to deploy
