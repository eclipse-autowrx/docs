---
title: "Your First Application"
description: "Create your first SDV application with SDV-Runtime"
date: 2025-05-05
draft: false
weight: 22
---

## Reading Guide

1. First, make sure you've [installed SDV-Runtime](../installation/)
2. On this page, learn how to create and test your first application
3. After creating your first app, explore the [Reference](/reference/) section for advanced topics
4. If you encounter issues, check the [Troubleshooting](/reference/troubleshooting/) section

## Step 2: Connecting to the Playground

The playground.digital.auto platform provides a web-based interface for developing SDV applications.

1. Open your web browser and navigate to [playground.digital.auto](https://playground.digital.auto)
2. Look for the runtime selector (usually in the top navigation bar)
3. Find and select your runtime named "MyFirstSDVRuntime" from the dropdown menu
4. You should see a confirmation that you're connected to your SDV-Runtime instance

## Step 3: Exploring Vehicle Signals

Before developing an application, it's helpful to understand the available vehicle signals.

1. In the playground interface, navigate to the VSS Explorer or Signal Browser section
2. Browse through the hierarchical tree of vehicle signals
3. Click on signals to view their details (type, description, etc.)
4. Take note of signals you might want to use in your application (e.g., `Vehicle.Body.Lights.IsLowBeamOn`)

## Step 4: Creating Your First Application

Let's create a simple application that toggles the vehicle's headlights.

1. In the playground, navigate to the Code Editor or Application section
2. Create a new Python file for your application
3. Add the following code:

```python
import asyncio
from sdv_model import Vehicle

# Initialize the vehicle model
vehicle = Vehicle()

async def toggle_headlights():
    # Print the current state
    current_state = await vehicle.Body.Lights.IsLowBeamOn.get()
    print(f"Current headlight state: {current_state}")
    
    # Toggle the state
    new_state = not current_state
    await vehicle.Body.Lights.IsLowBeamOn.set(new_state)
    print(f"Headlight state changed to: {new_state}")

# Run the async function
asyncio.run(toggle_headlights())
```

4. Save the file with a descriptive name (e.g., `headlight_toggle.py`)

## Step 5: Testing Your Application

Now that you've created your application, it's time to test it.

1. In the playground, find and click the "Run" or "Execute" button
2. Observe the console output showing the current headlight state and the change
3. If the playground has a visual interface showing the vehicle state, you should also see the headlight indicator change

## Working with More Signals

Most real-world applications interact with multiple vehicle signals. The Velocitas Python SDK included in SDV-Runtime makes this easy with its object-oriented API:

```python
import asyncio
from sdv_model import Vehicle

# Initialize the vehicle model
vehicle = Vehicle()

async def control_multiple_signals():
    # Read current temperature
    cabin_temp = await vehicle.Cabin.HVAC.AmbientAirTemperature.get()
    print(f"Current cabin temperature: {cabin_temp}°C")
    
    # Control air conditioning
    if cabin_temp > 24:
        print("Activating air conditioning")
        await vehicle.Cabin.HVAC.IsAirConditioningActive.set(True)
        await vehicle.Cabin.HVAC.Fan.Speed.set(50)
    else:
        print("Cabin temperature is comfortable")
    
    # Check and adjust lights based on ambient light
    ambient_light = await vehicle.Body.Lights.AmbientLight.get()
    if ambient_light < 30:
        print("Low ambient light, turning on headlights")
        await vehicle.Body.Lights.IsLowBeamOn.set(True)

# Run the function
asyncio.run(control_multiple_signals())
```

## Subscribing to Signal Changes

In addition to reading and writing signals, you can subscribe to be notified when signal values change:

```python
import asyncio
from sdv_model import Vehicle

# Initialize the vehicle model
vehicle = Vehicle()

# Define callback function
def on_speed_change(data):
    print(f"Vehicle speed changed to: {data.value} km/h")

async def monitor_speed():
    # Subscribe to speed changes
    vehicle.Speed.subscribe(on_speed_change)
    
    # Keep the program running
    await asyncio.sleep(60)  # Monitor for 60 seconds

# Run the monitoring function
asyncio.run(monitor_speed())
```

---

## Navigation

**Previous**: [← Installation Details](../installation/) | **Next**: [Technical Reference →](../reference/)

[Back to Documentation Home](/)
