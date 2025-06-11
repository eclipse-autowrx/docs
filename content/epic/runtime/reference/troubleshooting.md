---
title: "Troubleshooting"
description: "Troubleshooting guide for common SDV-Runtime issues"
date: 2025-05-05
draft: false
weight: 43
---

## Reading Guide

1. First, check if your issue is addressed in one of the sections below
2. Follow the step-by-step troubleshooting instructions for your specific issue
3. If your issue persists, refer to the debug tools section
4. For deployment-related issues, see the [Advanced Deployment](../deployment/) page

## Common Issues and Solutions

### 1. Databroker Connection Failures

```bash
# Check if databroker is running
ps aux | grep databroker

# Test gRPC connectivity
grpcurl -plaintext localhost:55555 list

# Check logs
journalctl -u kuksa-databroker
```

### 2. Mock Provider Not Generating Data

```python
# Debug mock provider
import logging
logging.basicConfig(level=logging.DEBUG)

# Check provider status
curl http://localhost:8080/provider/status
```

### 3. Syncer Connection Issues

```bash
# Test connectivity to kit manager
curl -v https://kit.digitalauto.tech/api/v1/status

# Check syncer logs
docker logs <container_id> 2>&1 | grep syncer
```

## Health Checks and Monitoring

The following can help you proactively identify and diagnose issues with SDV-Runtime:

### Health Checks

```python
# health_check.py
from flask import Flask, jsonify
import grpc

app = Flask(__name__)

@app.route('/health')
def health():
    checks = {
        'databroker': check_databroker(),
        'syncer': check_syncer(),
        'kit_manager': check_kit_manager()
    }
    
    status = 'healthy' if all(checks.values()) else 'unhealthy'
    return jsonify({'status': status, 'checks': checks})

def check_databroker():
    try:
        channel = grpc.insecure_channel('localhost:55555')
        grpc.channel_ready_future(channel).result(timeout=5)
        return True
    except:
        return False
```

### Logging Configuration

```python
# logging_config.py
LOGGING_CONFIG = {
    'version': 1,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'formatter': 'detailed'
        },
        'file': {
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': '/var/log/sdv-runtime.log',
            'maxBytes': 10485760,  # 10MB
            'backupCount': 5,
            'formatter': 'detailed'
        }
    },
    'formatters': {
        'detailed': {
            'format': '%(asctime)s %(levelname)s [%(name)s:%(lineno)d] %(message)s'
        }
    },
    'root': {
        'level': 'INFO',
        'handlers': ['console', 'file']
    }
}
```

### Debug Tools

```bash
# gRPC debugging
export GRPC_VERBOSITY=DEBUG
export GRPC_TRACE=all

# Network debugging
docker run --rm --network container:<container_id> nicolaka/netshoot

# Process monitoring
docker stats <container_id>
docker top <container_id>
```

---

## Navigation

**Previous**: [← Configuration Options](../configuration/) | **Next**: [Advanced Deployment →](../deployment/)
