---
title: "Instance Setup Guide"
linkTitle: "Instance Setup"
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
FRONTEND_PORT=3000
API_URL=http://localhost:3000/api
WS_URL=ws://localhost:3000

BACKEND_PORT=3000
NODE_ENV=production
MONGODB_URL=mongodb://mongodb:27017/autowrx

JWT_SECRET=your-secure-jwt-secret-key
JWT_ACCESS_EXPIRATION_MINUTES=30
JWT_REFRESH_EXPIRATION_DAYS=30

MONGODB_PORT=27017
```

## Docker Compose Setup

Create a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  frontend:
    image: ghcr.io/eclipse-autowrx/autowrx-app:v2.1.0.rc0
    ports:
      - "${FRONTEND_PORT:-3000}:4173"
    environment:
      - VITE_API_URL=${API_URL}
      - VITE_WS_URL=${WS_URL}
    depends_on:
      - backend
    restart: unless-stopped

  backend:
    image: ghcr.io/eclipse-autowrx/playground-be:dev-953d16d-v0.0.0
    ports:
      - "${BACKEND_PORT}:3000"
    environment:
      - NODE_ENV=${NODE_ENV}
      - MONGODB_URL=${MONGODB_URL}
      - JWT_SECRET=${JWT_SECRET}
      - JWT_ACCESS_EXPIRATION_MINUTES=${JWT_ACCESS_EXPIRATION_MINUTES}
      - JWT_REFRESH_EXPIRATION_DAYS=${JWT_REFRESH_EXPIRATION_DAYS}
    depends_on:
      - mongodb
    restart: unless-stopped

  sdv-runtime:
    image: ghcr.io/eclipse-autowrx/sdv-runtime:latest
    environment:
      - RUNTIME_NAME=ProductionRuntime
      - SYNCER_SERVER_URL=http://backend:3000
    ports:
      - "55555:55555"
    depends_on:
      - backend
    volumes:
      - ./data/runtime:/data
    restart: unless-stopped

  mongodb:
    image: mongo:4.4
    ports:
      - "${MONGODB_PORT}:27017"
    volumes:
      - ./data/mongodb:/data/db
    restart: unless-stopped
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

## Access the Services

- Frontend: http://localhost:3000  
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
