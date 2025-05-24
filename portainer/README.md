# Portainer

Portainer is a lightweight management UI that allows you to easily manage your Docker containers, images, networks, and volumes via a web interface.

## Service Info

- **Image**: `portainer/portainer-ce:lts`
- **Ports**:
  - `9443`: HTTPS Web UI
- **Data**: Stored in `./data` and mapped to `/data` inside the container

## Usage

Start Portainer:

```bash
docker compose up -d
````

Stop Portainer:

```bash
docker compose down
```

## Access

* [https://your-server-ip:9443](https://your-server-ip:9443)
* Create an admin user on first launch.

## Folder Structure

```
portainer/
├── docker-compose.yml
├── data/                # Persistent Portainer data
└── README.md
```

## Notes

* Make sure `docker.sock` is securely managed — it grants full control over Docker.
* Consider putting Portainer behind a reverse proxy with HTTPS and authentication for extra protection.

## Cleanup

To remove all Portainer data:

```bash
docker compose down -v
```

> ⚠️ This deletes everything (users, stacks, settings).
