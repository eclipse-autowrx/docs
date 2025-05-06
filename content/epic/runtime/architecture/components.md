---
title: "Component Details"
description: "Detailed information about SDV-Runtime components"
date: 2025-05-05
draft: false
weight: 11
---



## Reading Guide

1. First, make sure you've read the [Architecture Overview](../)
2. On this page, learn about each component in detail
3. Next, explore the [Data Flows](../data-flows/) to understand how components interact
4. After that, check the [Getting Started Guide](/getting-started/) to begin using the components

## Core Components

### KUKSA Databroker (v0.4.4)

The Databroker is the central component that handles vehicle data management with these key features:

* Maintains the current state of all vehicle signals
* Processes get, set, and subscribe operations
* Validates data against VSS specifications
* Provides a gRPC API for communication

**Key Characteristics**:
- **Protocol**: gRPC
- **Port**: 55555
- **Implementation**: Written in Rust for performance and safety
- **Data Format**: Uses Protocol Buffers for efficient data serialization

### Kuksa Syncer

The Syncer manages communication between the SDV-Runtime and the playground.digital.auto platform:

* Registers the runtime with the Kit Manager
* Maintains heartbeat monitoring
* Synchronizes configuration between systems
* Forwards events between systems

**Key Characteristics**:
- **Implementation**: Python-based synchronization service
- **Communication**: WebSocket protocol for real-time updates
- **Authentication**: Uses RUNTIME_NAME as identifier
- **Data Flow**: Bidirectional sync of vehicle signals

### Kit Manager

The Kit Manager handles runtime orchestration and management:

* Provides REST API for external communication
* Manages runtime lifecycle
* Handles configuration updates
* Provides status monitoring

**Key Characteristics**:
- **API**: REST API on port 3090 (in local mode)
- **Function**: Runtime registration and management
- **Integration**: Connects to playground.digital.auto

### Mock Provider

The Mock Provider simulates vehicle systems by providing data to the Databroker:

* Simulates various vehicle signals with configurable parameters
* Updates signals at configurable frequencies (default: 100ms)
* Provides realistic data patterns for testing

**Key Characteristics**:
- **Implementation**: Python-based mock data provider
- **Configuration**: Customizable signal ranges and update frequencies
- **Integration**: Directly communicates with Databroker

### Vehicle Signal Specification (VSS v4.0)

The Vehicle Signal Specification provides a standardized way to describe vehicle signals:

* Hierarchical organization of signals (e.g., Vehicle.Powertrain.Engine.Speed)
* Typed signals with metadata
* Standardized naming conventions

**Key Characteristics**:
- **Format**: JSON schema defining vehicle signals
- **Organization**: Tree structure with branches for different vehicle domains
- **Signal Types**: Sensors (read), Actuators (write), Attributes (static)

### Velocitas SDK (v0.14.1)

The SDK provides a developer-friendly interface for interacting with vehicle signals:

* Object-oriented model of the vehicle based on VSS
* Asynchronous methods for getting and setting values
* Subscription mechanisms for receiving updates
* Batch operations for efficiency

**Key Characteristics**:
- **Language**: Python SDK
- **Interface**: Provides a high-level API for applications
- **Implementation**: Communicates with Databroker using gRPC

## Component Interactions

The components work together through standardized interfaces, primarily:

1. **gRPC** - For communication with the Databroker
2. **REST** - For the Kit Manager's external API
3. **WebSockets** - For real-time communication with the playground

For details on how these components exchange data, see the [Data Flows](../data-flows/) page.

---

## Navigation

**Previous**: [← Architecture Overview](../) | **Next**: [Data Flows →](../data-flows/)

[Back to Documentation Home](/)
