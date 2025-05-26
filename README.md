# Home Server Setup

This repository contains a collection of self-hosted services deployed using Docker Compose on Ubuntu Server 24.04.

All services are organized in folders under `~/server/`, with each service having:
- Its own `docker-compose.yml`
- Optional `.env` file
- A dedicated `README.md` for setup and usage

Before starting any services, make sure to create the shared Docker network:

```bash
docker network create core-network
```

> **Note**: Secrets and volume data are excluded from version control via `.gitignore`.

## Available Services

### [MongoDB](./mongodb)
A containerized MongoDB 8.0 instance for development and local service usage.

- Exposes port `27017`
- Root credentials loaded from `.env`
- Data persisted in `mongodb/data`

### [Nginx Proxy](./nginx-proxy)
An automated reverse proxy with HTTPS support using Let's Encrypt DNS-01 challenges via Cloudflare.

- Exposes ports `80` and `443`
- Automatically issues and renews SSL certificates
- Certificates and ACME data stored in `nginx-proxy/certs` and `nginx-proxy/acme`
- Proxies any container using `VIRTUAL_HOST` and `LETSENCRYPT_HOST` environment variables

### [PiHole](./pihole)
A self-hosted network-wide DNS server and ad blocker.

- Exposes DNS on `53/tcp` and `53/udp`
- Admin interface reverse-proxied via `pihole.yourdomain.com`
- Data persisted in `pihole/etc-pihole` and `pihole/etc-dnsmasq.d`

### [Portainer](./portainer)
A web-based management UI for Docker. Manage containers, volumes, networks, and more.

- Reverse-proxied via `portainer.yourdomain.com`
- Data stored in `portainer/data`
- Docker socket mounted for full Docker control

### [Redis](./redis)
A password-protected Redis 7 instance for caching or local storage.

- Exposes port `6379`
- Password set via `.env`
- Data persisted in `redis/data`

### [Vaultwarden](./vaultwarden)
A self-hosted Bitwarden-compatible password manager.

- Reverse-proxied via `vault.yourdomain.com`
- Data persisted in `vaultwarden/data`
- No exposed ports — served via `core-network` and Nginx proxy

### [WireGuard VPN](./wireguard)
A secure VPN server to access your home network remotely.

- Exposes UDP port `51820`
- Peer configs and keys stored in `wireguard/config`
- Accessible via your domain (`wireguard.emilhornlund.com`)

## Getting Started

1. Clone this repository on your server:

```bash
git clone git@github.com:youruser/server.git ~/server
cd ~/server
