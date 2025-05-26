# Vaultwarden

This service runs Vaultwarden — a lightweight, self-hosted alternative to Bitwarden — using the official `vaultwarden/server` Docker image.

## Service Info

- **Image**: `vaultwarden/server:1.33.2-alpine`
- **Port**: Internally runs on `80`, exposed only to reverse proxy
- **Data Directory**: `./data` → `/data` inside the container (stores vault data and configuration)
- **Network**: Attached to the shared external `core-network`

## Usage

Start the service:

```bash
docker compose up -d
````

Stop the service:

```bash
docker compose down
```

## Configuration

Environment variables are defined in the `.env` file:

```env
TZ=Europe/Stockholm
DOMAIN=https://vault.yourdomain.com
VIRTUAL_HOST=vault.yourdomain.com
LETSENCRYPT_HOST=vault.yourdomain.com
VIRTUAL_PORT=80
```

> These environment variables are used by the reverse proxy to route traffic and issue SSL certificates via Let's Encrypt.

## Folder Structure

```
vaultwarden/
├── docker-compose.yml
├── .env.example
├── data/                # Vaultwarden persistent storage
└── README.md
```

## Networking

* Runs behind a reverse proxy at `vault.yourdomain.com`
* Internal traffic over port `80`
* No ports are exposed directly to the host
* Connects to the shared `core-network`

## Cleanup

To remove all Vaultwarden data:

```bash
docker compose down -v
```

> ⚠️ This deletes all user accounts, vault data, and configuration files.
