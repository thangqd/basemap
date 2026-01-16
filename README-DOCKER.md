# OpenMapTiles Styles HTTP Server

This setup provides an HTTP server to serve the OpenMapTiles styles using Docker and nginx.

## Quick Start

### Using Docker Compose (Recommended)

```bash
# Option 1: Modern Docker Compose (Docker Desktop v2.0+)
docker compose up -d
docker compose logs -f
docker compose down

# Option 2: Legacy docker-compose (if installed separately)
docker-compose up -d
docker-compose logs -f
docker-compose down
```

### Using Docker directly

```bash
# Build the image
docker build -t openmaptiles-styles .

# Run the container
docker run -d -p 8080:80 --name openmaptiles-styles openmaptiles-styles

# Stop and remove
docker stop openmaptiles-styles
docker rm openmaptiles-styles
```

## Access

Once running, access the styles at:
- **Main page**: http://localhost:8080
- **Style files**: http://localhost:8080/{style-name}/{style-name}.json

Available styles:
- http://localhost:8080/bright/bright.json
- http://localhost:8080/dark/dark.json
- http://localhost:8080/positron/positron.json
- http://localhost:8080/toner/toner.json
- http://localhost:8080/fiord/fiord.json
- http://localhost:8080/basic/basic.json
- http://localhost:8080/liberty/liberty.json
- http://localhost:8080/terrain/terrain.json
- http://localhost:8080/3d/3d.json

## Features

- **CORS enabled** for cross-origin requests
- **Gzip compression** for better performance
- **Security headers** included
- **Hot reload** in development (with volume mount)

## Development

For development with live reload:

```bash
# Use volume mount for live changes
docker run -d -p 8080:80 -v $(pwd)/styles:/usr/share/nginx/html:ro openmaptiles-styles
```

## Configuration

- **nginx.conf**: Custom nginx configuration with CORS and security headers
- **docker-compose.yml**: Container orchestration
- **.dockerignore**: Excludes unnecessary files from build context
- **.gitignore**: Git ignore patterns