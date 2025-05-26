# Portainer

Portainer is a lightweight management UI that allows you to easily manage your Docker containers, images, networks, and volumes via a web interface.

## Service Info

- **Image**: `portainer/portainer-ce:lts`
- **Port**: Internally runs on `9000`, exposed only to reverse proxy
- **Data Directory**: `./data` → `/data` inside the container (persists user settings and stacks)

## Usage

Start Portainer:

```bash
docker compose up -d
````

Stop Portainer:

```bash
docker compose down
```

## Folder Structure

```
portainer/
├── docker-compose.yml
├── data/                # Persistent Portainer data
└── README.md
```

## Networking

* Runs behind a reverse proxy at `portainer.yourdomain.com`
* Internal traffic over port `9000`
* No ports are exposed directly to the host
* Connects to the shared `core-network`

## Cleanup

To remove all Portainer data:

```bash
docker compose down -v
```

> ⚠️ This deletes everything (users, stacks, settings).
