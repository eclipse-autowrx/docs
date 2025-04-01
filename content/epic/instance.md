---
title: "Instance"
date: 2025-04-01T11:56:57+07:00
draft: false
weight: 7
---

### Setting Up Your Own Playground Instance

## Overview

The **playground.digital.auto** is an open, web-based prototyping environment designed for developing **Software-Defined Vehicle (SDV)** solutions.

The playground consists of two primary components:

- **Frontend**: [GitLab autowrx frontend](https://github.com/eclipse-autowrx/autowrx)
- **Backend**: [GitLab autowrx backend-core](https://github.com/eclipse-autowrx/backend-core/backend-core)

## Installation Requirements

It is recommended to use **Linux or macOS** for launching an instance. While the frontend works on both Windows and Linux/macOS, the backend currently **only supports Linux/macOS**.

### Installing Node.js

Node.js is required to run the application. Install **Node.js (>= 20.12.12)**:

```sh
nvm install 20.12.12  # If using Node Version Manager (NVM)
nvm use 20.12.12
```

Additionally, install [Yarn](https://yarnpkg.com/) for package management:

```sh
npm install -g yarn
```

### Installing Docker & Docker Compose

Docker is required to containerize and manage multiple instances. Install it following this guide:

- **Ubuntu 20.04**: [How to Install Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
- **MacOS**: [Install Docker Desktop on Mac](https://docs.docker.com/desktop/setup/install/mac-install/)

Verify installation:

```sh
docker --version
docker compose version
```

## Cloning the Source Code

### Install Git

Git is used for version control. Install it if you haven't already:

```sh
sudo apt install git  # Ubuntu
brew install git      # macOS
```

### Fork and Clone the Repositories

1. **Fork the repositories**:

   - [autowrx frontend](https://github.com/eclipse-autowrx/autowrx/-/forks/new)
   - [backend-core](https://github.com/eclipse-autowrx/backend-core/-/forks/new)

   > **_NOTES:_** Forking the repositories is optional but required if you plan to contribute changes to the project.

2. **Clone your forked repositories**:

   **Frontend:**

   ```sh
   gh repo clone eclipse-autowrx/autowrx
   cd autowrx
   yarn install  # Install dependencies
   ```

   **Backend:**

   ```sh
   gh repo clone eclipse-autowrx/backend-core
   cd backend-core
   yarn install  # Install dependencies
   ```

## Configuring Environment Variables

Before running the application, set up environment variables in **.env** files.

### **Frontend**

Check `.env.sample` in the `autowrx` folder for all required variables.

### **Backend**

Refer to `.env.sample` in the `backend-core` folder. The backend requires more environment variables related to external services such as:

- **Brevo** (Email sending)
- **GitHub** (Proposal Wishlist API in COVESA repository)
- **AWS** (GenAI request handling via AWS Bedrock)
- **ETAS** (ETAS GenAI model integration)
- **Certivity** (Vehicle API regulation data retrieval)

## Running the Application

### Development Mode

#### **Frontend**

```sh
cd autowrx
yarn dev
```

#### **Backend**

The backend runs as multiple containers (4 in total). Start it using:

```sh
cd backend-core
chmod +x ./start.sh
./start.sh
```

> **Note:** The backend may require `sudo` privileges to create directories for storing uploaded files.

### Production Mode

#### **Frontend**

There are two deployment options:

**Option 1 (Recommended): Manual Deployment**

```sh
cd autowrx
yarn build  # Creates a `dist` folder
pm2 serve dist 4000 --spa --name autowrx # Example command if you are using pm2 to deploy
```

**Option 2: Docker Deployment**

Choose this when you want to share and deploy multiple instances.

```sh
cd autowrx
yarn build
docker build -t <your_docker_username>/playground-fe:latest .
docker login # login to docker hub
docker push <your_docker_username>/playground-fe:latest
```

To run it:

```sh
export APP_PORT=<your_frontend_port>
export IMAGE_TAG=<your_docker_username>/playground-fe:latest
docker compose up -d --remove-orphans
```

> **Note:** If you modify the `.env` file, rebuild the Docker image before restarting.

#### **Backend**

```sh
cd backend-core
chmod +x ./start.sh
./start.sh -prod -d  # Runs in production mode (detached)
```

## Common Issues & Fixes

### **1. Node.js Version Not Compatible**

If you see an error related to an incompatible Node.js version, use **nvm** to switch. For example:

```sh
nvm install 20.12.12
nvm use 20.12.12
```

### **2. `docker compose` command not found**

This error may occur if you have installed `docker-compose` (with a hyphen), which is the older Python-based version. The backend currently uses the newer plugin-based version: `docker compose` (without a hyphen).

**Solution:**

- Replace `docker compose` with `docker-compose` in the `start.sh` script located in the `backend-core` folder.
- Alternatively, install the latest `docker compose` plugin to resolve the issue.

### **3. Missing Environment Variables**

Ensure you have set all required variables in `.env` files. Check `.env.sample` in both frontend and backend projects.

### **4. Unable to Log In After Successful Setup**

This issue may be caused by an incorrect `JWT_COOKIE_DOMAIN` environment variable. The value of this variable must match your backend's domain.

**Examples:**

- If your backend is running at `http://localhost:9800`, set `JWT_COOKIE_DOMAIN` to either `localhost` or `.`.
- If your backend is hosted at `https://domain.example.com`, set `JWT_COOKIE_DOMAIN` to `.`, `example.com`, or `domain.example.com`.

Ensure this setting is correctly configured to avoid authentication issues.
