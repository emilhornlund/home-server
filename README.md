# Home Server Setup

This repository contains a collection of self-hosted services deployed using Docker Compose on Ubuntu Server 24.04.

All services are organized in folders under `~/server/`, with each service having:
- Its own `docker-compose.yml`
- Optional `.env` file
- A dedicated `README.md` for setup and usage

> **Note**: Secrets and volume data are excluded from version control via `.gitignore`.

## Available Services

### [Portainer](./portainer)
A web-based management UI for Docker. Manage containers, volumes, networks, and more.

- HTTPS port: `9443`
- Data stored in `portainer/data`
- Docker socket mounted for full Docker control

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
