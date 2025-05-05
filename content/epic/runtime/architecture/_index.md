---
title: "SDV-Runtime Architecture"
description: "Overview of the SDV-Runtime architecture and design principles"
date: 2025-05-05
draft: false
weight: 10
---

The SDV-Runtime provides a containerized environment for Software Defined Vehicle (SDV) development that seamlessly integrates with the digital.auto playground platform. This page explains the high-level architecture and components of the SDV-Runtime system.

## Reading Guide

1. **Start here** to understand the overall architecture
2. Next, explore the [Component Details](./components/) to learn about each part
3. Then, read about the [Data Flows](./data-flows/) to understand how data moves through the system
4. Finally, head to the [Getting Started Guide](/getting-started/) to begin using SDV-Runtime

## System Overview

The SDV-Runtime architecture consists of multiple layers and components that work together to provide a complete environment for SDV application development. The architecture is designed to be flexible, scalable, and easy to deploy in various environments.

## Container Components

The SDV-Runtime container consists of several key components organized in a layered architecture:

### Runtime Environment Layer

- **Python 3.10 Runtime Environment**: The base runtime environment that supports all other components.

### Core Services Layer

- **KUKSA Databroker (gRPC:55555)**: The central data management component that provides access to vehicle signals through a gRPC interface on port 55555.
- **Kuksa Syncer**: Responsible for synchronizing data between the runtime and the playground.digital.auto platform.
- **Kit Manager**: Manages the runtime configuration and handles communication with the playground platform.

### Data Services Layer

- **Vehicle Signal Specification (VSS)**: Defines the standardized vehicle signals used by the system.
- **Mock Provider**: Simulates vehicle signals for testing and development.
- **Velocitas SDK**: Provides a developer-friendly API for interacting with vehicle signals.

## External Connections

- **playground.digital.auto**: Web-based platform for developing and testing SDV applications, connected to the Kit Manager.
- **External Applications**: Can connect directly to the KUKSA Databroker from outside the container.

---

## Navigation

**Previous**: [← Documentation Home](/) | **Next**: [Component Details →](./components/)

[Back to Documentation Home](/)
