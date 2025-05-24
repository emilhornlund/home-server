# WireGuard VPN

This service runs a self-hosted WireGuard VPN server using the `lscr.io/linuxserver/wireguard` Docker image. It allows secure remote access to your home network from anywhere.

## Service Info

- **Image**: `lscr.io/linuxserver/wireguard:latest`
- **Port**: `51820/udp`
- **Config path**: `./config` (binds to `/config` inside container)
- **Peers**: Generates configuration for 1 client

## Usage

Start the VPN server:

```shell
docker compose up -d
````

Stop it:

```shell
docker compose down
```

## Folder Structure

```
wireguard/
├── docker-compose.yml
├── config/              # Stores WireGuard keys, peer configs, and server config
└── README.md
```

## Client Configuration

After starting the container, you can retrieve the peer config from:

```shell
cat ./config/peer1/peer1.conf
```

You can scan the QR code for mobile:

```shell
docker exec -it wireguard cat /config/peer1/peer1.png
```

> Import the `.conf` into your desktop client or scan the QR with the WireGuard mobile app.

## DNS & Access

* Ensure UDP port `51820` is forwarded on your router to your server.
* The VPN server advertises itself as `wireguard.emilhornlund.com` — ensure DNS is configured correctly (e.g. Cloudflare DDNS).
* Internal VPN subnet: `10.13.13.0/24`

## Resetting or Adding Peers

To regenerate or add peers:

1. Stop the container
2. Remove `./config` if needed
3. Set `PEERS=3` (or desired number) in `docker-compose.yml`
4. Restart the container

This will regenerate fresh server and peer configurations.

> ⚠️ Removing `./config` will erase your existing VPN setup.
