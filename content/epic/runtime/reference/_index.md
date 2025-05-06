---
title: "Technical Reference"
description: "Technical reference documentation for SDV-Runtime"
date: 2025-05-05
draft: false
weight: 40
---



This section provides detailed technical information about SDV-Runtime.

## Reading Guide

1. Start here for an overview of the reference documentation
2. Choose a specific area to learn more about:
   - [API Documentation](./api-documentation/) - For developers integrating with SDV-Runtime
   - [Configuration Options](./configuration/) - For administrators configuring SDV-Runtime
   - [Troubleshooting](./troubleshooting/) - For solving common issues
   - [Advanced Deployment](./deployment/) - For production deployments

## Component Specifications

| Component | Version | API | Description |
|-----------|---------|-----|-------------|
| KUKSA Databroker | 0.4.4 | gRPC | Central data management system |
| Velocitas SDK | 0.14.1 | Python | Vehicle application SDK |
| Vehicle Signal Specification | 4.0 | JSON | Standardized signal definitions |
| Kuksa Syncer | - | REST/WS | Synchronization with playground |
| Kit Manager | - | REST | Runtime management |

## Version Compatibility Matrix

| SDV Runtime | KUKSA Databroker | VSS | Velocitas SDK | Python |
|-------------|------------------|-----|---------------|--------|
| 1.0.0       | 0.4.4           | 4.0 | 0.14.1        | 3.10   |
| 0.9.0       | 0.4.0           | 3.0 | 0.13.0        | 3.9    |
| 0.8.0       | 0.3.1           | 3.0 | 0.12.0        | 3.9    |

---

## Navigation

**Previous**: [← Your First Application](/getting-started/first-application/) | **Next**: [API Documentation →](./api-documentation/)

[Back to Documentation Home](/)
