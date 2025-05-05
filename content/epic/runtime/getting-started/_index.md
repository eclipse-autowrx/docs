---
title: "Getting Started with SDV-Runtime"
description: "Quick start guide for SDV-Runtime"
date: 2025-05-05
draft: false
weight: 20
---

This guide will help you quickly set up SDV-Runtime and create your first vehicle application.

## Reading Guide

1. Start here for a quick overview of SDV-Runtime setup
2. Next, check [Installation Details](./installation/) for detailed setup instructions
3. Then, follow the [First Application](./first-application.md) guide to build your first app
4. After that, explore the [Reference](./reference/) section for in-depth information

## Overview

SDV-Runtime is a containerized environment for Software Defined Vehicle (SDV) development that provides:

* Pre-installed and configured SDV components
* Integration with the playground.digital.auto platform
* Tools for developing and testing vehicle applications

## Quick Installation

1. Ensure you have Docker installed on your system
2. Run the following command:

```bash
docker run -d -e RUNTIME_NAME="MyFirstSDVRuntime" ghcr.io/eclipse-autowrx/sdv-runtime:latest
```

3. Navigate to [playground.digital.auto](https://playground.digital.auto) and select your runtime

## Development Workflow

A typical development workflow with SDV-Runtime includes:

1. **Start the SDV-Runtime container** using the command above
2. **Connect to playground.digital.auto** and select your runtime
3. **Explore available vehicle signals** through the VSS browser
4. **Develop your application** using the Python editor
5. **Test your application** within the playground environment

---

## Navigation

**Previous**: [← Data Flows](/architecture/data-flows/) | **Next**: [Installation Details →](./installation/)

[Back to Documentation Home](/)
