---
title: "Data Flow Patterns"
description: "Understanding data and control flows in SDV-Runtime"
date: 2025-05-05
draft: false
weight: 12
---



Understanding the flow of data through the SDV-Runtime architecture is essential for effective development. This page explains the key data flows and control patterns.

## Reading Guide

1. First, make sure you've read the [Architecture Overview](../) and [Component Details](../components/)
2. On this page, learn how data flows between components
3. After understanding the architecture, proceed to the [Getting Started Guide](/getting-started/) to begin using SDV-Runtime

## Main Component Interactions

The following describes the main data flows between components:

### 1. Signal Publishing Flow

**Pattern**: Mock Provider → KUKSA Databroker → Subscribed Applications

**Description**:
1. The Mock Provider generates simulated vehicle signals (e.g., Vehicle.Speed = 50 km/h)
2. These signals are published to the KUKSA Databroker based on the VSS specification
3. Applications that have subscribed to these signals receive updates when values change

### 2. Management Flow

**Pattern**: Kit Manager ↔ Kuksa Syncer ↔ playground.digital.auto

**Description**:
1. The Kit Manager initializes and manages the runtime environment
2. The Kuksa Syncer establishes connection with playground.digital.auto
3. Configuration, status, and commands are exchanged bidirectionally
4. The playground can send commands and configuration updates back to the runtime

### 3. Application Data Access Flow

**Pattern**: Application → Velocitas SDK → KUKSA Databroker → VSS Data

**Description**:
1. Vehicle applications use the Velocitas SDK's object-oriented API
2. The SDK translates these requests to Databroker API calls
3. The Databroker retrieves or updates the requested data
4. Results are returned through the SDK to the application

### 4. Python Runtime Connections

**Pattern**: Python Runtime → All Components

**Description**:
- The Python runtime environment supports all components
- It provides direct access to the Mock Provider, Velocitas SDK, KUKSA Databroker, and Kuksa Syncer
- It enables communication between various services through standard Python interfaces

## Detailed Data Flow Examples

### Example 1: Reading a Vehicle Signal

1. An application requests the current vehicle speed via the Velocitas SDK:
   ```python
   speed = await vehicle.Speed.get()
   ```

2. The Velocitas SDK translates this to a gRPC request to the Databroker:
   ```
   GetRequest(path="Vehicle.Speed")
   ```

3. The Databroker looks up the current value and returns it:
   ```
   GetResponse(value=50, timestamp=...)
   ```

4. The SDK processes the response and returns the value to the application:
   ```python
   # speed now contains 50
   ```

### Example 2: Subscribing to Signal Changes

1. An application subscribes to speed changes:
   ```python
   vehicle.Speed.subscribe(on_speed_change)
   ```

2. The SDK registers this subscription with the Databroker:
   ```
   SubscribeRequest(path="Vehicle.Speed")
   ```

3. When the Mock Provider updates the speed value:
   ```
   SetRequest(path="Vehicle.Speed", value=55)
   ```

4. The Databroker sends an update to all subscribers:
   ```
   SubscriptionUpdate(path="Vehicle.Speed", value=55)
   ```

5. The SDK receives this update and calls the application's callback function:
   ```python
   on_speed_change(SpeedData(value=55, timestamp=...))
   ```

## Flow Categories

The data flows in SDV-Runtime can be categorized as:

- **Signal Flow**: Related to vehicle signal data generation and distribution
- **Management Flow**: Handling runtime configuration and external communication
- **Application Flow**: Supporting application development and interaction

---

## Navigation

**Previous**: [← Component Details](../components/) | **Next**: [Getting Started →](/getting-started/)

[Back to Documentation Home](/)
