# 🚀 Deployment Guide

How GameSeek is deployed on the production VPS.

-----

## Server

- **OS:** Debian Server 
- **Domain:** gameseekapp.xyz 
- **Reverse Proxy:** Nginx
- **SSL:** Let’s Encrypt

-----

## Nginx Configuration

GameSeek runs two WebSocket endpoints simultaneously:

|Endpoint                   |Used by          |
|---------------------------|-----------------|
|`wss://gameseekapp.xyz/ws` |Browser (Web App)|
|`ws://gameseekapp.xyz:8765`|Electron EXE     |

Example Nginx block for the WebSocket proxy:

```nginx
location /ws {
    proxy_pass http://localhost:8765;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
}
```

-----

## SSL / HTTPS

SSL is handled by Nginx + Let’s Encrypt (`certbot`).

```bash
certbot --nginx -d gameseekapp.xyz -d www.gameseekapp.xyz
```

-----

## SendGrid (Email)

- Domain authenticated via CNAME records on Porkbun
- SPF record configured for `gameseekapp.xyz`
- `FROM_EMAIL` set to `support@gameseekapp.xyz`

-----

## Starting the Backend

```bash
cd backend
nohup python server.py &
```

Or use a process manager like `systemd` or `pm2` for auto-restart.

-----

## Updating the App

```bash
git pull origin main
# Restart backend
pkill -f server.py
nohup python server.py &
# Rebuild frontend if needed
cd frontend && npm run build
```
