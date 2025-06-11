---
title: "API Documentation"
description: "Detailed API documentation for SDV-Runtime components"
date: 2025-05-05
draft: false
weight: 41
---

## Reading Guide

1. First, make sure you've read the [Technical Reference Overview](../)
2. On this page, learn about the APIs provided by SDV-Runtime components
3. For integration patterns, see the [Advanced Deployment](../deployment/#integration-patterns) section
4. For configuration options, check the [Configuration](../configuration/) page

## KUKSA.val Databroker gRPC API

**Proto Definition**:
```protobuf
service VAL {
  rpc Get(GetRequest) returns (GetResponse);
  rpc Set(SetRequest) returns (SetResponse);
  rpc Subscribe(SubscribeRequest) returns (stream SubscribeResponse);
  rpc GetMetadata(GetMetadataRequest) returns (GetMetadataResponse);
  rpc RegisterDatapoint(RegisterDatapointRequest) returns (RegisterDatapointResponse);
}

message DataEntry {
  string path = 1;
  Datapoint value = 2;
  Timestamp timestamp = 3;
}

message Datapoint {
  oneof value {
    string string = 1;
    bool bool = 2;
    int32 int32 = 3;
    int64 int64 = 4;
    uint32 uint32 = 5;
    uint64 uint64 = 6;
    float float = 7;
    double double = 8;
    StringArray string_array = 10;
    // ... other array types
  }
}
```

## Velocitas SDK API

**Client Initialization**:
```python
from velocitas_sdk import VehicleApp
from sdv_model import Vehicle

class MyVehicleApp(VehicleApp):
    def __init__(self):
        super().__init__()
        self.Vehicle = Vehicle()
```

**Data Access Patterns**:
```python
# Read data
speed = await self.Vehicle.Speed.get()

# Write data
await self.Vehicle.Cabin.Lights.IsGloveBoxOn.set(True)

# Subscribe to changes
await self.Vehicle.Speed.subscribe(self.on_speed_change)

# Batch operations
await self.Vehicle.update({
    'Cabin.Lights.IsDomeOn': True,
    'Cabin.Lights.AmbientLight': 50
})
```

## Additional API Resources

* [KUKSA Databroker GitHub Repository](https://github.com/eclipse-kuksa/kuksa-databroker)
* [Velocitas SDK Documentation](https://github.com/eclipse-velocitas/vehicle-app-python-sdk)
* [VSS Specification](https://github.com/COVESA/vehicle_signal_specification)

---

## Navigation

**Previous**: [← Technical Reference](./technical-reference.md) | **Next**: [Configuration Options →](./configuration/)

[Back to Documentation Home](/)
