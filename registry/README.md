# Registry

This service runs a private Docker registry with basic auth and HTTPS support.

## 📦 Service Info

- **Image**: `registry:2`
- **Port**: `5000`
- **Auth**: Basic HTTP auth using `htpasswd`
- **TLS**: Uses custom certificates located in `./certs/`

## 🛠 Usage

Start the registry:

```bash
docker compose up -d
````

Stop it:

```bash
docker compose down
```

## 🔐 Configuration

### Create Registry Credentials

To generate a basic authentication file for the registry:

```bash
htpasswd -Bbn myuser "SuperSecret!" > auth/htpasswd
```

> The file will be mounted into the container at `/auth/htpasswd`.

### Set Up TLS with a Self-Signed Certificate

This registry uses HTTPS with a custom certificate signed by a local Certificate Authority (CA). Follow these steps to generate and configure the certs.

#### 1. Create a Local Certificate Authority (CA)

Run on the host where the registry will run:

```bash
mkdir -p ~/registry-tls && cd ~/registry-tls

# Generate CA private key
openssl genrsa -out registry-ca.key 4096

# Generate public CA certificate (valid for 10 years)
openssl req -x509 -new -nodes \
  -key registry-ca.key \
  -sha256 -days 3650 \
  -subj "/CN=Registry Local CA" \
  -out registry-ca.crt
```

You now have:

* `registry-ca.crt` – Public CA certificate (to distribute to clients)
* `registry-ca.key` – Private key (keep secure, do not share)

#### 2. Issue a TLS Certificate for Your Hostname

Replace `emils-nuc-server` with your registry's hostname:

```bash
cat > registry.cnf <<'EOF'
[ req ]
default_bits       = 4096
prompt             = no
default_md         = sha256
distinguished_name = dn
req_extensions     = req_ext

[ dn ]
CN = emils-nuc-server

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = emils-nuc-server
EOF
```

Then run:

```bash
# Generate private key
openssl genrsa -out emils-nuc-server.key 4096

# Generate CSR
openssl req -new -key emils-nuc-server.key -out emils-nuc-server.csr -config registry.cnf

# Sign certificate with your CA
openssl x509 -req -in emils-nuc-server.csr \
  -CA registry-ca.crt -CAkey registry-ca.key -CAcreateserial \
  -out emils-nuc-server.crt -days 825 -sha256 -extensions req_ext -extfile registry.cnf
```

Place `emils-nuc-server.crt` and `emils-nuc-server.key` in `registry/certs/`.

#### 3. Trust the CA on All Docker Hosts and Runners

Each Docker host or GitHub runner must trust your CA:

```bash
sudo mkdir -p /etc/docker/certs.d/emils-nuc-server:5000
sudo cp registry-ca.crt /etc/docker/certs.d/emils-nuc-server:5000/ca.crt
sudo systemctl restart docker
```

> Be sure the hostname `emils-nuc-server` matches what Docker is using when you `docker push`.

## 📁 Folder Structure

```
registry/
├── docker-compose.yml
├── auth/                # Stores htpasswd credentials
├── certs/               # TLS certificate and key
├── data/                # Persistent image storage
└── README.md
```

## Networking

* Exposed on port `5000`
* Attached to the shared external `core-network`

## Cleanup

To remove all registry data:

```bash
docker compose down -v
```

> ⚠️ This deletes all pushed images and credentials.
