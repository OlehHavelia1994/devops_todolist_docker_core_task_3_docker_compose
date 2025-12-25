# INSTRUCTION.md

## Overview

This document provides step-by-step instructions for building, running, and stopping Docker containers using **docker-compose** for the Todo application and its MySQL database.

The application consists of two services:

* **todoapp** — Python-based application running on port 8080
* **mysql** — MySQL database service

Both services run in the same Docker bridge network and communicate internally.

---

## Prerequisites

Before starting, ensure the following software is installed on your system:

* Docker
* Docker Compose (v2 or higher)

Verify installation with the commands:

```bash
docker --version
docker compose version
```

---

## Project Structure

The project directory should contain at least the following files:

```
.
├── docker-compose.yml
├── Dockerfile
├── Dockerfile.mysql
└── INSTRUCTION.md
```

---

## Build and Run Containers

### 1. Build images and start containers

From the root of the project, run:

```bash
docker compose up --build
```

This command will:

* Build the **todoapp** image using `Dockerfile`
* Build the **mysql** image using `Dockerfile.mysql`
* Create the `app-net` bridge network
* Create the `db-volume` Docker volume
* Start both containers

The application will be available at:

```
http://localhost:8080
```

---

### 2. Run containers in detached mode (optional)

To start containers in the background, use:

```bash
docker compose up -d 
```

---

## Verify Running Containers

Check running containers:

```bash
docker compose ps
```

Check logs:

```bash
docker compose logs
```

Logs for a specific service:

```bash
docker compose logs todoapp
docker compose logs mysql
```

---

## Stop Containers

### 1. Stop running containers

```bash
docker compose down
```

This command will:

* Stop and remove containers
* Remove the network created by docker-compose

The **db-volume** will be preserved by default.

---

### 2. Stop containers and remove volumes (optional)

⚠️ This will delete all database data.

```bash
docker compose down -v
```

---

## Restart Containers

To restart containers after stopping them:

```bash
docker compose up -d
```

---

## Environment Variables

### todoapp

* `PYTHONUNBUFFERED=1` — ensures real-time logging output

### mysql

* `MYSQL_ROOT_PASSWORD=1234`
* `MYSQL_DATABASE=app_db`
* `MYSQL_USER=app_user`
* `MYSQL_PASSWORD=1234`

---

## Network and Volume Details

* **Network:** `app-net` (bridge)
* **Volume:** `db-volume` — persists MySQL data between container restarts

---

## Notes

* The `todoapp` service depends on the `mysql` service and will start after the database container is created.
* MySQL is exposed on port `3306` for local access if needed.
* Application port `8080` is mapped to the host machine.
