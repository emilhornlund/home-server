# NGINX Reverse Proxy

This service provides an automated reverse proxy using NGINX, with automatic Let's Encrypt certificates issued via DNS-01 challenges through Cloudflare.

## Service Info

- **Proxy Image**: `nginxproxy/nginx-proxy:alpine`
- **SSL Image**: `nginxproxy/acme-companion`
- **Ports**:
  - `80`: HTTP
  - `443`: HTTPS
- **Certs**: Stored in `./certs` and issued with `acme.sh` via Cloudflare DNS

## Usage

Start the reverse proxy:

```bash
docker compose up -d
```

Stop the reverse proxy:

```bash
docker compose down
```

## Folder Structure

```
nginx-proxy/
├── .env                 # DNS API credentials for Let's Encrypt (not committed)
├── docker-compose.yml   # Defines nginx and acme-companion services
├── certs/               # Issued TLS certificates
├── acme/                # ACME account and renewal configs
├── conf/
│   ├── vhost.d/         # Per-subdomain nginx config fragments
│   └── html/            # Default fallback HTML content
└── README.md
```

## Add Proxied Apps

To proxy a container through this reverse proxy:

1. Connect it to the `core-network`
2. Set environment variables in the container:

   * `VIRTUAL_HOST`: Subdomain (e.g. `app.example.com`)
   * `LETSENCRYPT_HOST`: Same as above (for certificate issuance)
   * `VIRTUAL_PORT`: Internal app port (e.g. `80`)

Example:

```yaml
environment:
  VIRTUAL_HOST: app.example.com
  LETSENCRYPT_HOST: app.example.com
  VIRTUAL_PORT: 80
networks:
  - core-network
```

## Notes

* Make sure subdomains exist in Cloudflare and are proxied (orange cloud).
* `.env` must contain a valid `ACMESH_DNS_API_CONFIG` block with `dns_cf` credentials.
* Certificates are automatically renewed.

## Cleanup

To remove the reverse proxy and all data:

```bash
docker compose down -v
```

> ⚠️ This deletes all issued certificates and ACME account data.
