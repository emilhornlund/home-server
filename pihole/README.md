# Pi-hole

This service runs Pi-hole as a local DNS server and network-wide ad blocker, using the official `pihole/pihole` Docker image.

## Service Info

- **Image**: `pihole/pihole:2025.04.0`
- **Ports**:
  - `53/tcp` and `53/udp` for DNS resolution
- **Data Directories**:
  - `./etc-pihole` → `/etc/pihole` (core Pi-hole data and DB)
  - `./etc-dnsmasq.d` → `/etc/dnsmasq.d` (custom DNS configuration)
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
WEBPASSWORD=your_pihole_admin_password
VIRTUAL_HOST=pihole.yourdomain.com
VIRTUAL_PORT=80
LETSENCRYPT_HOST=pihole.yourdomain.com
```

> The password is required to log in to the Pi-hole admin interface.

## Folder Structure

```
pihole/
├── docker-compose.yml
├── .env.example
├── etc-pihole/         # Persistent Pi-hole data
├── etc-dnsmasq.d/      # Custom DNS configs
└── README.md
```

## Networking

* DNS is exposed on `53` over both TCP and UDP.
* Web admin interface is expected to be reverse-proxied (e.g. `https://pihole.yourdomain.com`).
* Connects to the shared `core-network` for service discovery and proxy routing.

## Cleanup

To remove all Pi-hole data and configurations:

```bash
docker compose down -v
```

> ⚠️ This deletes all Pi-hole settings, query logs, and custom DNS records.
