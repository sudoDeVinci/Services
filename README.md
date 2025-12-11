# Services

A collection of self-hosted services managed w/ Docker Compose.

## Prerequisites

- Docker and Docker Compose
- Create the shared network for the DNS servers before starting lancache or pihole:
  ```
  docker network create --subnet=172.28.0.0/16 lancache-net
  ```
  
## Configuration

Each service with a `.env.example` file requires configuration before first run. Copy the example file to `.env` and edit the placeholder values.

## Network Layout

Lancache and Pi-hole share the `lancache-net` network (172.28.0.0/16). Pi-hole is assigned 172.28.0.10 and serves as the upstream DNS for Lancache.

## Services

### Immich

Photo and video management server with machine learning capabilities (configured for AMD ROCm since that's what I use).

| Port | Service |
|------|---------|
| 2283 | Web UI |

```
cd immich
cp .env.example .env
docker compose up -d
```

### Lancache

LAN caching server for game downloads. Works in conjunction with Pi-hole for DNS.

| Port | Service |
|------|---------|
| 53 | DNS |
| 80 | HTTP cache |
| 443 | HTTPS passthrough |

```
cd lancache
cp .env.example .env
docker compose up -d
```

### ownCloud

File sync and share platform.

| Port | Service |
|------|---------|
| 5045 (configurable) | Web UI |

```
cd owncloud
cp .env.example .env
docker compose up -d
```

### Pi-hole

Network-wide ad blocking DNS server. Configured to run on the lancache network as an upstream DNS resolver.

| Port | Service |
|------|---------|
| 8088 | Web UI (HTTP) |
| 8443 | Web UI (HTTPS) |
| 8053 | DNS |

```
cd pihole
cp .env.example .env
docker compose up -d
```

### Portainer

Docker management UI.

| Port | Service |
|------|---------|
| 9443 | Web UI (HTTPS) |

```
cd portainer
docker compose up -d
```

### Open WebUI

Web interface for Ollama. Requires Ollama running on the host at port 11434.

| Port | Service |
|------|---------|
| 3000 | Web UI |

```
cd webui
docker compose up -d
```

Firewall rule (if needed):
```
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
sudo firewall-cmd --reload
```
