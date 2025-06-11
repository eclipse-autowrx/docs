---
title: "Advanced Deployment"
description: "Advanced deployment options for SDV-Runtime"
date: 2025-05-05
draft: false
weight: 44
---

## Reading Guide

1. First, make sure you understand the [basic installation](/getting-started/installation/)
2. On this page, learn about advanced deployment options
3. For integration patterns, see the section below
4. For performance tuning, refer to the [Configuration Options](../configuration/) page

## Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sdv-runtime
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sdv-runtime
  template:
    metadata:
      labels:
        app: sdv-runtime
    spec:
      containers:
      - name: sdv-runtime
        image: ghcr.io/eclipse-autowrx/sdv-runtime:latest
        env:
        - name: RUNTIME_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        ports:
        - containerPort: 55555
          name: databroker
        - containerPort: 3090
          name: kit-manager
        resources:
          limits:
            memory: "2Gi"
            cpu: "2"
          requests:
            memory: "1Gi"
            cpu: "1"
        livenessProbe:
          httpGet:
            path: /health
            port: 3090
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          tcpSocket:
            port: 55555
          initialDelaySeconds: 5
          periodSeconds: 10
```

## Docker Swarm Stack

```yaml
version: '3.8'
services:
  sdv-runtime:
    image: ghcr.io/eclipse-autowrx/sdv-runtime:latest
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
    environment:
      - RUNTIME_NAME={{.Service.Name}}-{{.Task.Slot}}
    networks:
      - sdv-network
    
networks:
  sdv-network:
    driver: overlay
    attachable: true
```

## Multi-Runtime Orchestration

```python
# orchestrator.py
import docker
import time

class SDVRuntimeOrchestrator:
    def __init__(self):
        self.client = docker.from_env()
        self.runtimes = {}
    
    def spawn_runtime(self, name, config):
        container = self.client.containers.run(
            'ghcr.io/eclipse-autowrx/sdv-runtime:latest',
            detach=True,
            environment={
                'RUNTIME_NAME': name,
                **config
            },
            ports={'55555/tcp': None}  # Random port mapping
        )
        self.runtimes[name] = container
        return container
    
    def scale_runtimes(self, count):
        current = len(self.runtimes)
        if count > current:
            for i in range(current, count):
                self.spawn_runtime(f'runtime-{i}', {})
        elif count < current:
            # Scale down logic
            pass
```

## Integration Patterns

### 1. Direct gRPC Integration

```python
import grpc
from kuksa.val.v1 import val_pb2, val_pb2_grpc

channel = grpc.insecure_channel('localhost:55555')
stub = val_pb2_grpc.VALStub(channel)

# Get value
request = val_pb2.GetRequest(entries=[
    val_pb2.EntryRequest(path="Vehicle.Speed", fields=[val_pb2.Field.FIELD_VALUE])
])
response = stub.Get(request)
```

### 2. REST Proxy Pattern

```python
from flask import Flask, jsonify
import grpc

app = Flask(__name__)

@app.route('/api/vehicle/<path:signal_path>')
def get_signal(signal_path):
    # Forward to gRPC
    value = get_from_databroker(signal_path)
    return jsonify({'path': signal_path, 'value': value})
```

### 3. WebSocket Bridge

```javascript
const ws = new WebSocket('ws://localhost:8080/ws');

ws.onopen = () => {
    ws.send(JSON.stringify({
        action: 'subscribe',
        path: 'Vehicle.Speed'
    }));
};

ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    console.log(`Speed: ${data.value} km/h`);
};
```

---

## Navigation

**Previous**: [← Troubleshooting](../troubleshooting/) | **Next**: [SDV-Runtime technical reference →](./technical-reference.md)
