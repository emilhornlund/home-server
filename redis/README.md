# Redis

This service runs a password-protected Redis 7 instance using the official `redis` Docker image.

## Service Info

- **Image**: `redis:8.0.1`
- **Port**: `6379`
- **Data Directory**: `./data` → `/data` inside the container (persists all Redis data)
- **Network**: Attached to the shared external `core-network`
- **Authentication**: Password protected using the `REDIS_PASSWORD` environment variable

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

The `.env` file defines the Redis password:

```env
REDIS_PASSWORD=supersecretpassword
```

> Make sure to update this password and avoid committing real secrets.

## Folder Structure

```
redis/
├── docker-compose.yml
├── .env.example
├── data/                # Redis persistent storage
└── README.md
```

## Networking

* Exposed on port `6379` for local or container-based connections.
* Connects to the shared `core-network` for service discovery.

## Cleanup

To remove all Redis data:

```bash
docker compose down -v
```

> ⚠️ This deletes all keys and persistent storage in `data/`.
