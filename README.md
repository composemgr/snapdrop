# Snapdrop

A self-hosted snapdrop application.

## Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/snapdrop/main/docker-compose.yaml" | docker compose -f - up -d
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/snapdrop" ~/.local/srv/docker/snapdrop
cd ~/.local/srv/docker/snapdrop
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install snapdrop
```

## Configuration

See docker-compose.yaml for environment variables and configuration options.

## Documentation

Check the official project documentation for detailed setup and usage information.
