---
title: "Runtime"
description: "Documentation for the Software Defined Vehicle Runtime"
date: 2025-05-05
draft: false
weight: 1
---

The SDV-Runtime is a containerized environment designed to simplify the process of working with Software Defined Vehicle (SDV) technology. It provides a complete, pre-configured environment for running and developing SDV applications without requiring complex setup procedures.

## Reading Guide

Follow this step-by-step guide to understand and use the SDV-Runtime:

### Step 1: Understand the Basics
* [What is SDV-Runtime?](#overview) - Learn about the basic concepts
* [Key Features](#key-features) - Discover the main capabilities
* [Included Components](#included-components) - Explore what's included in the container

### Step 2: Set Up Your Environment 
* [Installation Guide](./getting-started/) - Install SDV-Runtime on your system
* [Configuration Options](./getting-started/installation/) - Configure the runtime for your needs

### Step 3: Learn the Architecture
* [Architecture Overview](./architecture/) - Understand how SDV-Runtime works
* [Component Details](./architecture/components/) - Learn about each component
* [Data Flows](./architecture/data-flows/) - Understand how data moves through the system

### Step 4: Develop Applications
* [Build Your First Application](./getting-started/first-application/) - Create a simple application
* [Working with Vehicle Signals](./getting-started/first-application/#working-with-more-signals) - Learn to use vehicle data

### Step 5: Advanced Topics
* [API Documentation](./reference/api-documentation/) - Detailed API information
* [Troubleshooting](./reference/troubleshooting/) - Solve common problems
* [Advanced Deployment](./reference/deployment/) - Deploy in production environments

## Overview

The SDV-Runtime is especially valuable for developers who are new to SDV technology and want a straightforward way to learn, experiment, and develop applications without dealing with the complexities of setting up multiple components individually.

## Key Features

- **Ready-to-use Docker container** with all essential SDV components pre-installed and configured
- **Native integration** with [playground.digital.auto](https://playground.digital.auto) for rapid ideation, development, and demonstration
- **Cross-platform support** for both `amd64` and `arm64` architectures, enabling deployment across various environments (cloud, PC, or Raspberry Pi)
- **Simple deployment** with minimal configuration requirements

## Included Components

The SDV-Runtime container includes the following pre-configured components:

| Component | Version | Description |
|-----------|---------|-------------|
| [Kuksa Databroker](https://github.com/eclipse-kuksa/kuksa-databroker) | 0.4.4 | Vehicle data management system that handles the communication between vehicle applications and vehicle signals |
| [Vehicle Signal Specification (VSS)](https://github.com/COVESA/vss-tools) | 4.0 | Standardized way to describe and communicate vehicle signals |
| [vehicle-model-generator](https://github.com/eclipse-velocitas/vehicle-model-generator) | v0.7.2 | Tool for generating vehicle models from VSS specifications |
| Kuksa Syncer | - | Tool for synchronizing data between the Kuksa Databroker and external systems |
| [Kuksa Mock Provider](https://github.com/eclipse-kuksa/kuksa-mock-provider) | - | Provides mock data to simulate vehicle signals (with modifications to the original) |
| [Velocitas Python SDK](https://github.com/eclipse-velocitas/vehicle-app-python-sdk) | v0.14.1 | SDK for developing vehicle applications in Python |
| Kit Manager | - | Tool for managing deployment and configuration of SDV components |
| [Python](https://www.python.org) | 3.10 | Programming language for developing SDV applications |

## Quick Start

```bash
docker run -d -e RUNTIME_NAME="MyRuntimeName" ghcr.io/eclipse-autowrx/sdv-runtime:latest
```

---

## Navigation

**Next**: [Getting Started →](./getting-started/)

[Back to Documentation Home](/)
