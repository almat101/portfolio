---
title: "Transcendence"
description: "Multiple docker containers"
date: "2025-03-20"
draft: false
tags: ["docker", "docker compose", "microservice", "django", "postgres","javascript", "bootstrap", "nginx", "Grafana", "Prometheus", "Elasticsearch", "Kibana", "Logstash"]
showToc: false
weight: 201
cover:
    image: "https://raw.githubusercontent.com/almat101/Transcendence/refs/heads/main/img/Screenshots/login_cut.png"
---
### 🔗 [Subject]()

# Transcendence

# Multi-Service Dockerized Application

This project is about creating a multi-service application orchestrated with Docker Compose. It integrates static frontend, backend microservices, advanced monitoring tools, and an ELK stack for centralized logging and analytics. The architecture emphasizes modularity, scalability, and seamless deployment, making it robust and efficient for modern application needs.


## Table of Contents

- [Overview](#overview)
- [Services](#services)
  - [Proxy](#proxy)
  - [Backend Services](#backend-services)
  - [Monitoring](#monitoring)
  - [ELK Stack](#elk-stack)
- [Setup](#setup)
- [Usage](#usage)
- [Volumes](#volumes)
- [Networks](#networks)


## Overview

This project is a Single Page Application (SPA) that replicates the classic Pong game with modern features and a robust backend architecture. It includes:

- **Secure Login**: User authentication and authorization for a personalized experience.
- **Tournament Management**: Organize and participate in tournaments with other players.
- **AI Opponent**: Play against an AI-powered opponent for solo gameplay.
- **Match History**: Track and review the history of all matches played.
- **Monitoring and Analytics**: Tools like Grafana, Prometheus, and Alertmanager for real-time monitoring and alerting.
- **Centralized Logging**: An ELK stack (Elasticsearch, Logstash, Kibana) for managing and visualizing logs.
- **Reverse Proxy**: An nginx proxy server for secure routing and load balancing.

The application is containerized using Docker and orchestrated with Docker Compose, ensuring scalability, modularity, and ease of deployment.

## Services

### Proxy

The **Proxy** service acts as a reverse proxy and includes an API Gateway implemented within Nginx. It provides the following features:

- **Image**: proxy
- **Ports**: `8090:8090`
- **Healthcheck**: Ensures the proxy is running and accessible.
- **Dependencies**: Relies on backend services (e.g., authentication, tournament, history), monitoring tools (e.g., Grafana, Prometheus), and the ELK stack.

The Nginx configuration includes API Gateway functionality, making it a lightweight yet effective API Gateway. It handles:

- **Routing**: Directs API requests to appropriate backend services based on the URL path (`/api/user/` to `auth-service`, `/api/tournament/` to `tournament-service` and  `/api/history/` to `history-service`).
- **Rate Limiting**: Implements request rate limiting using the `api_limit` zone to prevent abuse.
- **Authentication**: Validates JWT tokens for protected routes and extracts user information for authenticated requests.
- **Security**: Adds standard proxy headers and enforces HTTPS with strong SSL/TLS configurations.
- **Static File Serving**: Serves the frontend static files, which are copied into the container during the Docker build process, ensuring efficient delivery of the application's static assets.

### Backend Services

1. **Auth Service**
   - **Description**: Handles user authentication and authorization, including secure login and OAuth integration.
   - **Framework**: Built with Django.
   - **Health Check**: Exposes health checks at `/watchman/?skip=watchman.checks.storage`.

2. **History Service**
   - **Description**: Manages the storage and retrieval of match history, allowing users to review past games.
   - **Framework**: Built with Django.
   - **Health Check**: Exposes health checks at `/watchman/`.

3. **Tournament Service**
   - **Description**: Provides functionality for creating, managing, and participating in tournaments.
   - **Framework**: Built with Django.
   - **Health Check**: Exposes health checks at `/watchman/`.

4. **Databases**
   - **Description**: Each backend service is paired with its own PostgreSQL database to ensure data isolation and scalability.
   - **Services**:
     - `auth_db`: Stores user authentication and authorization data.
     - `history_db`: Stores match history data.
     - `tournament_db`: Stores tournament-related data.

### Monitoring

1. **Grafana**
   - **Description**: A powerful visualization and monitoring dashboard that provides insights into system and application performance.
   - **Features**:
     - Two pre-configured dashboards:
       1. **Django Dashboard**: Allows switching between all microservices (e.g., `auth-service`, `history-service`, `tournament-service`) to monitor metrics like request rates, response times, and errors.
       2. **Node Exporter Dashboard**: Displays detailed system metrics, including CPU usage, RAM utilization, disk space, and network activity.
   - **Access**: Available at `https://localhost/grafana/`.

2. **Prometheus**
   - **Description**: A metrics collection and monitoring system that scrapes data from various services and evaluates alerting rules.
   - **Features**:
     - Collects metrics from all microservices, PostgreSQL databases, and the Nginx proxy.
     - Evaluates alerting rules defined in rules_1.yml, rules_2.yml, rules_3.yml, and rules_4.yml.
   - **Healthcheck**: Endpoint available at `http://localhost:9090/-/healthy`.

3. **Node Exporter**
   - **Description**: A lightweight exporter that collects system-level metrics from the host machine.
   - **Features**:
     - Provides data on CPU usage, memory availability, disk space, and network performance.
     - Integrated with the Node Exporter Dashboard in Grafana for detailed visualization.

4. **Alertmanager**
   - **Description**: Manages alerts generated by Prometheus and routes them to configured notification channels.
   - **Features**:
     - Sends alerts via email for critical issues, such as service downtime, high CPU usage, low disk space, or memory exhaustion.
     - Configured with alerting rules for all microservices and system metrics.

5. **Postgres Exporters**
   - **Description**: Specialized exporters that collect metrics from PostgreSQL databases.
   - **Features**:
     - Monitors database health, active connections, query performance, replication lag, and disk usage.
     - Metrics are visualized in Grafana and used for alerting in Prometheus.

### Alerts and Notifications

- **Alert Rules**: Defined in rules_1.yml, rules_2.yml, rules_3.yml, and rules_4.yml to monitor critical conditions across all services and infrastructure.
  - Examples:
    - **Instance Down**: Alerts when any service or exporter is unreachable.
    - **High CPU Load**: Triggers when CPU usage exceeds 80% for more than 5 minutes.
    - **Low Disk Space**: Alerts when disk space falls below 10%.
    - **PostgreSQL Issues**: Monitors database downtime, high connection counts, slow queries, and replication lag.
    - **Nginx Proxy Down**: Alerts if the Nginx proxy becomes unreachable.
- **Notification**: Alerts are sent via email to ensure timely responses to critical issues.

### ELK Stack

1. **Elasticsearch**
   - **Description**: A distributed search and analytics engine that serves as the centralized storage for logs and analytics.
   - **Features**:
     - Stores and indexes logs from various sources, including Nginx `access.log`.
     - Provides powerful search and aggregation capabilities for log analysis.
   - **Security**:
     - Fully secured with SSL and authentication.
     - Certificates for Elasticsearch are generated by a dedicated `setup` container, which creates all necessary certificates for Elasticsearch and Kibana.
     - Configured to use HTTPS for all communications, ensuring data integrity and privacy.
   - **Integration**: Works seamlessly with Logstash for log ingestion and Kibana for visualization.

2. **Logstash**
   - **Description**: A data processing pipeline that ingests, transforms, and forwards logs to Elasticsearch.
   - **Features**:
     - Parses Nginx `access.log` files and enriches the data for analysis.
     - Configured to securely communicate with Elasticsearch using SSL certificates generated by the `setup` container.
   - **Role in the Stack**:
     - Acts as the intermediary between Nginx logs and Elasticsearch, ensuring logs are properly formatted and indexed for analysis.

3. **Kibana**
   - **Description**: A visualization and exploration tool for Elasticsearch data.
   - **Features**:
     - Provides a pre-configured dashboard to analyze Nginx `access.log` data, including request rates, response statuses, client IPs, and more.
     - Allows users to query and visualize Elasticsearch data with ease.
   - **Access**: Available at `https://localhost/kibana`.
   - **Security**:
     - Uses SSL for secure communication with Elasticsearch.
     - Certificates for secure communication are generated by the `setup` container.
   - **Dashboards**:
     - Includes a dedicated dashboard for analyzing all aspects of the Nginx `access.log`, providing insights into traffic patterns, error rates, and client activity.

### Additional Notes

- **Setup Container**:
  - A dedicated container (`setup`) is responsible for generating all SSL certificates required by Elasticsearch and Kibana.
  - Ensures that all components of the ELK stack communicate securely using HTTPS.

- **Nginx Access Logs**:
  - The ELK stack is configured to parse and analyze Nginx `access.log` files.
  - Kibana's dashboard provides detailed insights into Nginx traffic, including request patterns, error rates, and client activity.

- **Secure Communication**:
  - All components of the ELK stack (Elasticsearch, Logstash, and Kibana) are configured to use SSL for secure communication, ensuring data privacy and integrity.

