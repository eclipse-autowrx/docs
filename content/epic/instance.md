---
title: "Instance"
linkTitle: "Instance"
weight: 20
description: >
  Comprehensive guide for setting up Eclipse AutoWRX components: Frontend, Backend, and SDV Runtime using Docker
---

## Overview

Eclipse AutoWRX consists of three main components that work together:

1. **Frontend (autowrx)**: React-based web interface for the digital.auto playground  
2. **Backend (backend-core)**: RESTful API server handling business logic  
3. **SDV Runtime**: Containerized runtime environment for vehicle applications  

This guide covers the setup and configuration of all components using Docker only.

## Prerequisites

- Docker & Docker Compose  
- Git  
- Linux/macOS recommended  
- GitHub Personal Access Token (PAT) for pulling GHCR images  

## Environment Configuration

Create a `.env` file:

```env
# Environment
NODE_ENV=development
ENV=dev

# MongoDB
MONGODB_URL=mongodb://mongodb:27017/playground-be
MONGO_USER=
MONGO_PASSWORD=
MONGO_DB=playground
MONGODB_PORT=27017
MONGOOSE_STRICT_QUERY=true
DB_CONTAINER_NAME=${ENV}-playground-db

# Upload Service
UPLOAD_PORT=9810
UPLOAD_DOMAIN=/api/v2/file/
UPLOAD_PROTOCOL=http
UPLOAD_PATH=./data/dev/upload

# Application Ports
PORT=8080
KONG_PROXY_PORT=9800
KONG_NGINX_WORKER_PROCESSES=auto

# JWT
JWT_SECRET=examplesecret
JWT_ACCESS_EXPIRATION_MINUTES=30
JWT_REFRESH_EXPIRATION_DAYS=30
JWT_COOKIE_NAME=token

# Homologation
HOMOLOGATION_AUTH_CLIENT_ID=your_client_id
HOMOLOGATION_AUTH_CLIENT_SECRET=your_client_secret

# CORS
CORS_ORIGIN=localhost:\d+,127\.0\.0\.1:\d+

# Admin Configuration
ADMIN_EMAILS=admin@example.com
ADMIN_PASSWORD=admin_secure_password

# Logging
LOG_LEVEL=debug
```

## Docker Compose Setup

Create a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  frontend:
    image: ghcr.io/eclipse-autowrx/autowrx-app:latest
    ports:
      - "${FRONTEND_PORT:-3000}:4173"
    environment:
      - VITE_API_URL=${API_URL}
      - VITE_WS_URL=${WS_URL}
    depends_on:
      - backend
    restart: unless-stopped

  backend:
    image: ghcr.io/eclipse-autowrx/playground-be:dev-131e939-v0.1.0
    ports:
      - "${BACKEND_PORT}:3000"
    environment:
      - NODE_ENV=${NODE_ENV}
      - MONGODB_URL=${MONGODB_URL}
      - JWT_SECRET=${JWT_SECRET}
      - JWT_ACCESS_EXPIRATION_MINUTES=${JWT_ACCESS_EXPIRATION_MINUTES}
      - JWT_REFRESH_EXPIRATION_DAYS=${JWT_REFRESH_EXPIRATION_DAYS}
      - UPLOAD_PORT=${UPLOAD_PORT}
      - UPLOAD_DOMAIN=${UPLOAD_DOMAIN}
    depends_on:
      - mongodb
    restart: unless-stopped
    entrypoint: []
    command: ["yarn", "dev"]

  sdv-runtime:
    image: ghcr.io/eclipse-autowrx/sdv-runtime:latest
    environment:
      - RUNTIME_NAME=ProductionRuntime
      - SYNCER_SERVER_URL=http://localhost:3090
    ports:
      - "55555:55555"
    depends_on:
      - backend
    volumes:
      - ./data/runtime:/data
    restart: unless-stopped

  mongodb:
    image: mongo:4.4.6-bionic
    ports:
      - "${MONGODB_PORT}:27017"
    volumes:
      - dbdata:/data/db
    restart: unless-stopped

volumes:
  dbdata:
```

## Start the Stack

1. Authenticate to GitHub Container Registry:

```bash
echo $GITHUB_PAT | docker login ghcr.io -u USERNAME --password-stdin
```

2. Pull and start all services:

```bash
docker compose pull
docker compose up -d
```

## Troubleshooting Backend DNS Error

If after starting the stack you see the following error in the backend logs:

```
backend-1      | error: Error: getaddrinfo ENOTFOUND api.github.com
```

You can fix this by disabling the scheduled check in the backend container.

### Manual Fix

1. Access the backend container shell (replace `test-backend-1` with your actual container name):

```bash
docker exec -it test-backend-1 sh
```

2. Edit the backend source file:

```bash
vi /usr/src/playground-be/src/index.js
```

3. Locate and comment out the last line:

```js
// setupScheduledCheck();
```

4. Save and exit the editor.

5. Restart the backend container:

```bash
docker restart test-backend-1
```

### Quick Fix Command

Alternatively, run this single command (replace `test-backend-1` with your backend container name if different):

```bash
docker exec -it test-backend-1 sh -c "sed -i '\$s/^setupScheduledCheck();/\/\/ setupScheduledCheck();/' /usr/src/playground-be/src/index.js" && docker restart test-backend-1
```

This will comment out the scheduled check line and restart the backend container in one step.

## Access the Services

- Frontend: http://localhost:4173  
- Backend API: http://localhost:3000/api  
- SDV Runtime Databroker: localhost:55555  

## Logs and Status

```bash
docker compose ps
docker compose logs -f
```

## Support and Resources

- [Frontend Issues](https://github.com/eclipse-autowrx/autowrx/issues)  
- [Backend Issues](https://github.com/eclipse-autowrx/backend-core/issues)  
- [SDV Runtime Issues](https://github.com/eclipse-autowrx/sdv-runtime/issues)