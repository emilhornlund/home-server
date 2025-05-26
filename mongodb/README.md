# MongoDB

This service runs a MongoDB 8.0 container for local development or self-hosted applications.

## Service Info

- **Image**: `mongo:8.0.9`
- **Port**: `27017`
- **Data Directory**: `./data` → `/data/db` inside the container (persists databases)
- **Network**: Attached to the shared external `core-network`

## 🛠 Usage

Start the service:

```bash
docker compose up -d
````

Stop the service:

```bash
docker compose down
```

## Authentication

Default credentials are defined in the `.env` file:

```env
MONGO_INITDB_ROOT_USERNAME=root
MONGO_INITDB_ROOT_PASSWORD=password
```

> Do **not** use these credentials in production. Modify the `.env` or use secrets in secure environments.

## Folder Structure

```
mongodb/
├── docker-compose.yml
├── .env.example         # Sample environment configuration
├── data/                # MongoDB persistent data
└── README.md
```

## Networking

* Exposed on `27017` for local or container-based connections.
* Connects to the shared `core-network` for internal communication between containers.

## 🧼 Cleanup

To remove all MongoDB data:

```bash
docker compose down -v
```

> ⚠️ This deletes everything, including databases, users, and configuration stored in `data/`.
