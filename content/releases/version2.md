---
title: "Migrate from v1 to v2"
date: 2023-11-15T20:50:38+07:00
draft: false
weight: 1
---

At [playground.digital.auto](https://playground.digital.auto), we've listened to your feedback and identified some limitations in the architecture of v1. To address these, we're excited to announce a new version with significant improvements!
- Python applications utilize the standard Velocitas library. 
- Python applications can leverage external libraries, enabling the development of a wide range of applications.
- Widgets are fully plug-and-play, with no dependency on any specific model.
- UX improvements enhance user-friendliness.
- Rust Language Support: Write, compile, and execute Rust code with a single click. Supports 100 popular Rust libraries by default.
- New Feature: Staging replaces the previous Deploy dialog, simplifying the management of deployments from the development environment to target hardware.
- SDV Runtime: A Docker image serves as the execution environment for SDV applications, accommodating both Python and Rust applications. Users can either utilize numerous shared runtimes provided by us or deploy their own runtime on their local machine or in the cloud.
- Asset Management: Users can add and manage their own SDV runtimes or hardware kits, ensuring exclusive usage and controlled sharing. Furthermore, users can share their assets with collaborators.

To accommodate the aforementioned features, the underlying architecture of the entire playground has undergone a complete overhaul. Please take note of the following points when transitioning to version 2.

## 1. The Python code is formatted differently now

Below is sample code for simple head light blinking app.

{{< columns >}} <!-- begin columns block -->
#### Version 1
```python
from sdv_model import Vehicle
import plugins
from browser import aio

vehicle = Vehicle()

while True:
    await vehicle.Body.Lights.Beam.Low.IsOn.set(True)
    await aio.sleep(1)
    await vehicle.Body.Lights.Beam.Low.IsOn.set(False)
    await aio.sleep(1)
```

<---> <!-- magic separator, between columns -->

#### Version 2
```python
from vehicle import Vehicle
import time
import asyncio
import signal

from sdv.vdb.reply import DataPointReply
from sdv.vehicle_app import VehicleApp
from vehicle import Vehicle, vehicle

class TestApp(VehicleApp):

    def __init__(self, vehicle_client: Vehicle):
        super().__init__()
        self.Vehicle = vehicle_client

    async def on_start(self):
        while True:
            await asyncio.sleep(1)
            await self.Vehicle.Body.Lights.Beam.Low.IsOn.set(True)
            
            await asyncio.sleep(1)
            await self.Vehicle.Body.Lights.Beam.Low.IsOn.set(False)
            await asyncio.sleep(1)

async def main():
    vehicle_app = TestApp(vehicle)
    await vehicle_app.run()


LOOP = asyncio.get_event_loop()
LOOP.add_signal_handler(signal.SIGTERM, LOOP.stop)
LOOP.run_until_complete(main())
LOOP.close()
```

{{< /columns >}}

As you may have noticed, version 1 simplified the code style to facilitate a quick understanding of the Vehicle API concept. However, this simplified approach precluded execution on actual hardware. In version 2, we strictly adhere to the Velocitas library and templates, enabling code execution on real hardware. This bridges the gap between the playground and hardware setup. The code you write and run in the playground will now accurately reflect the behavior on the target hardware.

On version 2:
- The app is now a class that inherit standard class VehicleApp.
- Your code will be trigger from function: `async def on_start(self):`
- Vehicle API set/get/subcrible is followed below formated:
```python
# write an actuator signal with value
await self.Vehicle.Body.Lights.Beam.Low.IsOn.set(True)

# read an actuator back
value = (await self.Vehicle.Body.Lights.Beam.Low.IsOn.get()).value
print("Light value ", value)

...
# subscibe for signal value change
async def on_light_value_changed(self, data: DataPointReply):
    print("Detect that light value changed")
    new_value = data.get(self.Vehicle.Body.Lights.Beam.Low.IsOn).value
    print("Value change to", new_value)
async def on_start(self):
    await self.Vehicle.Body.Lights.Beam.Low.IsOn.subscribe(self.on_light_value_changed)
...

```

## 2. The plugin mechanism has been deprecated
To enhance clarity and adhere to the plug-and-play principle, widgets are now decoupled from any specific model. Python code and widgets operate with complete isolation.

[widgets] <=> Vehicle API <=> [SDV App]

Data exchange between widgets and your application must exclusively occur through the Vehicle API.


## 3. Your Python code runs within SDV Runtime, which is a Docker container
Upon clicking the Run button, your code is executed within a runtime environment. The runtime processes your code and streams the output back to the dashboard. Consequently, it is crucial to carefully select the appropriate runtime environment before running your application.

We provide a selection of shared runtimes for user convenience. These pre-configured environments allow users to readily execute their applications. Alternatively, users have the flexibility to configure and deploy their own custom runtime environments.





