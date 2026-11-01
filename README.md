## 👋 Welcome to Snapdrop 🚀

Self-hosted file sharing that works in your browser

## 📋 Description

Snapdrop is a self-hosted file sharing application where the transfer happens
directly between browsers over WebRTC, with no accounts and no server-side
storage in the middle. The linuxserver image wraps Pairdrop, the maintained
fork of the original Snapdrop project, and serves the web UI on port 80.

Transfers are ephemeral, a share exists only while at least one browser holds
the room open. The config volume holds the Pairdrop instance settings; uploaded
files are never written to the host.

## 🚀 Services

- **app**: lscr.io/linuxserver/pairdrop:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/snapdrop/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/snapdrop" ~/.local/srv/docker/snapdrop
cd ~/.local/srv/docker/snapdrop
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install snapdrop
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
SERVICE_USER=1000
SERVICE_GROUP=1000
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64346` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Both proxy variants need WebSocket support, which the defaults provide. WebRTC
itself is peer to peer and does not traverse the proxy.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64346

## 📂 Volumes

- `./volumes/config/snapdrop` - Pairdrop configuration, generated on first start

## 🔐 Security

- There is no authentication, anyone who can reach the URL can start a transfer
  with anyone else who visits the same page
- Set `SERVICE_USER` and `SERVICE_GROUP` to the host user that should own the
  config volume
- Use HTTPS via the reverse proxy in production, browsers block camera,
  microphone and clipboard access on plain HTTP origins, and WebRTC requires a
  secure context outside `localhost`
- The container is published only on the dockerhost address `172.17.0.1`
- Regularly update Docker images for security patches

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
