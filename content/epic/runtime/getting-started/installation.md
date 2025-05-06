---
title: "Installation Details"
description: "Detailed installation instructions for SDV-Runtime"
date: 2025-05-05
draft: false
weight: 21
---



## Prerequisites

Before you begin, ensure you have:

- Docker installed on your system ([Docker installation guide](https://docs.docker.com/get-docker/))
- A web browser for accessing the playground.digital.auto platform
- Basic knowledge of Python programming

## Reading Guide

1. First, make sure you've read the [Getting Started Overview](../)
2. On this page, learn how to install and configure SDV-Runtime
3. Next, follow the [First Application](../first-application/) guide to create your first app
4. If you encounter issues, check the [Troubleshooting](/reference/troubleshooting/) section

## Step 1: Installing SDV-Runtime

SDV-Runtime is distributed as a Docker container, making it easy to install and run on any system that supports Docker.

1. Open a terminal or command prompt
2. Run the following command to pull and start the SDV-Runtime container:

```bash
docker run -d -e RUNTIME_NAME="MyFirstSDVRuntime" ghcr.io/eclipse-autowrx/sdv-runtime:latest
```

3. Verify that the container is running:

```bash
docker ps
```

You should see a container with the image name `ghcr.io/eclipse-autowrx/sdv-runtime:latest` in the list.

## Advanced Installation Options

### Accessing the Kuksa Databroker

If you need to interact with the Kuksa Databroker from outside the container (e.g., using the kuksa-client), add port forwarding to your run command:

```bash
docker run -d -e RUNTIME_NAME="MyRuntimeName" -p 55555:55555 ghcr.io/eclipse-autowrx/sdv-runtime:latest
```

This exposes port 55555, allowing you to connect to the Databroker from your host machine.

### Configuring the Runtime Manager Server

By default, the SDV-Runtime connects to the [kit.digitalauto.tech](https://kit.digitalauto.tech) server. If you need to use a different server, you can specify it with the `SYNCER_SERVER_URL` environment variable:

```bash
docker run -d -e RUNTIME_NAME="MyRuntimeName" -e SYNCER_SERVER_URL="YOUR_SERVER" ghcr.io/eclipse-autowrx/sdv-runtime:latest
```

### Running with a Local Self Manager

For local development or when you don't want to connect to external servers, you can run with a local self manager:

```bash
docker run -d -e RUNTIME_NAME="MyRuntimeName" -e SYNCER_SERVER_URL="http://localhost:3090" -p 3090:3090 ghcr.io/eclipse-autowrx/sdv-runtime:latest
```

This configuration keeps everything in the localhost, with no external connections required.

## Building the Container

For most users, pulling the pre-built container from the GitHub Container Registry is sufficient. However, if you need to modify the container or build it for a specific environment, you can build it yourself.

### Prerequisites for Building

- Docker with BuildKit support
- Git to clone the repository

### Building Multi-Architecture Images

To build a multi-architecture image (both `amd64` and `arm64`):

```bash
# Create a builder instance
docker buildx create --driver docker-container --name mybuilder --platform "linux/amd64,linux/arm64" default

# Use the builder
docker buildx use mybuilder

# Build and push the image
docker buildx build --push --platform linux/amd64,linux/arm64 -t your-registry/sdv-runtime:tag -f Dockerfile .
```

Note: The `--push` option is required for multi-architecture builds with BuildKit.

### Building Single-Architecture Images

For a single-architecture build:

```bash
# Use the default builder
docker buildx use default

# Build the image
docker buildx build --platform linux/amd64 -t sdv-runtime:latest -f Dockerfile .
```

---

## Navigation

**Previous**: [← Getting Started](../) | **Next**: [Your First Application →](../first-application/)

[Back to Documentation Home](/)
