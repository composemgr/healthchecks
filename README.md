## 👋 Welcome to healthchecks 🚀

A simple, self-hosted monitoring tool for cron jobs and services

## 📋 Description

healthchecks.io is a cron job and service monitoring tool. It pings you when
something goes wrong: a scheduled job that misses its expected run, a service
endpoint that stops answering, or a backup that stops completing.

## 🚀 Services

- **app**: healthchecks/healthchecks:latest

### Infrastructure Components

- **healthchecks-db**: postgres:latest - Postgres database


## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/healthchecks/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/healthchecks" ~/.local/srv/docker/healthchecks
cd ~/.local/srv/docker/healthchecks
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install healthchecks
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_SECRET_KEY=replace-with-a-random-32-byte-hex-string
DB_CREATE_DATABASE_NAME=healthchecks
DB_USER_NAME=healthchecks
DB_USER_PASS=replace-with-a-random-password
EMAIL_SERVER_HOST=172.17.0.1
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
| `docker-compose.yaml` | Default, published on `172.17.0.1:64197` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64197

## 📂 Volumes

- `./volumes/db/postgres/healthchecks` - Database data

## 🔐 Security

- Change all default passwords before deploying to production
- `APP_SECRET_KEY` signs sessions; changing it logs every user out
- Postgres is internal-only and never published to the host
- Check ping URLs are unguessable — they are bearer credentials
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your database regularly

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

# Create an administrator
docker compose --env-file .env exec app python manage.py createsuperuser
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
