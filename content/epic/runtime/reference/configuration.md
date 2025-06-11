---
title: "Configuration Options"
description: "Configuration options for SDV-Runtime components"
date: 2025-05-05
draft: false
weight: 42
---

## Reading Guide

1. First, make sure you've read the [Technical Reference Overview](../)
2. On this page, learn about the configuration options for SDV-Runtime
3. For security-related configuration, see the security section below
4. If you encounter issues, check the [Troubleshooting](../troubleshooting/) page

## Container Resource Limits

```yaml
# docker-compose.yml
version: '3.8'
services:
  sdv-runtime:
    image: ghcr.io/eclipse-autowrx/sdv-runtime:latest
    deploy:
      resources:
        limits:
          cpus: '2.0'
          memory: 2G
        reservations:
          cpus: '1.0'
          memory: 1G
```

## Databroker Optimization

```toml
[performance]
max_connections = 1000
thread_pool_size = 16
cache_size = "256MB"
batch_size = 100

[grpc]
max_receive_message_size = 16777216  # 16MB
max_send_message_size = 16777216     # 16MB
```

## Network Optimization

```bash
# Use host network for better performance
docker run --network=host ...

# Or create optimized bridge network
docker network create --driver bridge --opt com.docker.network.bridge.enable_icc=true sdv-net
```

## Security Configuration

### TLS Configuration

```bash
# Generate certificates
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout server.key -out server.crt

# Mount certificates
docker run -v $(pwd)/certs:/etc/kuksa/certs:ro \
  -e KUKSA_TLS_ENABLED=true \
  -e KUKSA_TLS_CERT=/etc/kuksa/certs/server.crt \
  -e KUKSA_TLS_KEY=/etc/kuksa/certs/server.key \
  ghcr.io/eclipse-autowrx/sdv-runtime:latest
```

### Authentication Setup

```python
# JWT token validation
import jwt

def validate_token(token):
    try:
        payload = jwt.decode(token, PUBLIC_KEY, algorithms=['RS256'])
        return payload['scope'] == 'sdv.runtime.full'
    except jwt.InvalidTokenError:
        return False
```

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| RUNTIME_NAME | Identifier for the runtime in the playground | (required) |
| SYNCER_SERVER_URL | URL of the server for synchronization | https://kit.digitalauto.tech |
| DATABROKER_PORT | Port for the Kuksa Databroker | 55555 |
| LOG_LEVEL | Logging verbosity (DEBUG, INFO, WARNING, ERROR) | INFO |

---

## Navigation

**Previous**: [← API Documentation](../api-documentation/) | **Next**: [Troubleshooting →](../troubleshooting/)

[Back to Documentation Home](/)
