# Docker Multi‑Service Setup

This document describes how to create a shared Docker network and run multiple services (MySQL, Redis, API, etc.) using Docker Compose.

## 1. Requirements

- Docker

- Docker Compose (plugin docker compose)

Quick check:

```bash
docker --version
docker compose version
```

## 2. Create a Shared Network

All services will join the same network so they can communicate with each other using service names.

```bash
docker network create app-network
```

> You only need to run this once. If the network already exists, Docker will return an error, but it won’t cause any issues.

## 3. Directory Structure (Example)

```css
.
├── mysql/
│ └── docker-compose.yml
├── redis/
│ └── docker-compose.yml
└── README.md
```

Each service has its own directory and an independent docker-compose.yml file.

## 4. Start Individual Services

Example: MySQL

```bash
cd mysql
docker compose up -d
```

## 5. Network Convention in docker-compose

In each `docker-compose.yml`, services should use the shared `app-network`:

```bash
networks:
  app-network:
    external: true
```

Example:

```bash
services:
  mysql:
    image: mysql:8.0
    networks:
      - app-network

networks:
  app-network:
    external: true
```

## 6. Stop Services

From the corresponding service directory:

```bash
docker compose down
```

## Notes

- Services can connect to each other using **service names** (e.g. `mysql`, `redis`) instead of `localhost`.

- Each service can be run **independently**, or multiple services can run in parallel.

- This README can be extended as new services are added.
