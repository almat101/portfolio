---
title: "Inception"
description: "Multiple docker containers"
date: "2024-11-15"
draft: false
tags: ["docker", "docker compose", "redis", "mariadb", "wordpress", "nginx", "ftp", "hugo"]
showToc: false
weight: 202
cover:
    image: "/logo/inception.png"
---
### 🔗 [Subject]()

# Inception

This project aims to broaden your knowledge of system administration by using Docker. You will virtualize several Docker images, creating them in your new personal virtual machine.

## Project Overview

Inception involves setting up and configuring multiple services using Docker containers, each serving a specific purpose. The project uses Docker Compose to manage the containers, networks, and volumes required.

## Containers

### Nginx
- **Description**: Acts as a reverse proxy server to handle HTTP and HTTPS requests.
- **Configuration**: Configured to use a self-signed SSL certificate.
- **Ports**: Exposes port 443 for HTTPS.
- **Dependencies**: Depends on the WordPress container.
- **Build Context**: `requirements/nginx`

### MariaDB
- **Description**: A relational database management system to store WordPress data.
- **Configuration**: Uses a custom configuration file.
- **Ports**: Exposes port 3306 for database connections.
- **Build Context**: `requirements/mariadb`

### WordPress
- **Description**: A content management system to create and manage websites.
- **Configuration**: Automatically configured using a script.
- **Ports**: Exposes port 9000 for PHP-FPM.
- **Dependencies**: Depends on the MariaDB and Redis containers.
- **Build Context**: `requirements/wordpress`

### Redis
- **Description**: An in-memory data structure store used as a cache for WordPress.
- **Configuration**: Uses a custom configuration file.
- **Ports**: Exposes port 6379 for Redis connections.
- **Build Context**: `requirements/bonus/redis`

### FTP
- **Description**: An FTP server to manage file uploads.
- **Configuration**: Uses a custom configuration file and script.
- **Ports**: Exposes ports 21 and 20 for FTP, and a range of ports for passive mode.
- **Dependencies**: Depends on the WordPress container.
- **Build Context**: `requirements/bonus/ftp`

### Hugo
- **Description**: A static site generator to create a portfolio website.
- **Configuration**: Uses a custom configuration file.
- **Ports**: Exposes port 1313 for the Hugo server.
- **Build Context**: `requirements/bonus/hugo`

### Adminer
- **Description**: A lightweight database management tool.
- **Configuration**: Accessed via the Nginx container for security.
- **Ports**: Exposes port 9001 for internal access.
- **Build Context**: requirements/bonus/adminer

### Prometheus
- **Description**: A monitoring tool for collecting and analyzing metrics from services and containers.
- **Configuration**: Uses a custom configuration file to monitor specific metrics.
- **Ports**: Exposes port 9090 for accessing the Prometheus web interface.
- **Build Context**: requirements/bonus/prometheus

### Node Exporter
- **Description**: An exporter for Prometheus to collect hardware and OS metrics from the host machine.
- **Ports**: Exposes port 9100 for internal communication with Prometheus.
- **Build Context**: requirements/bonus/node-exporter

### Grafana
- **Description**: A visualization tool to create dashboards using metrics collected by Prometheus.
- **Ports**: Exposes port 3000 for accessing the Grafana web interface.
- **Build Context**: requirements/bonus/grafana

## Docker Compose Configuration

The project uses Docker Compose to manage the containers. The configuration file is located at `srcs/docker-compose.yml`. It defines the services, networks, and volumes required for the project.

### Networks
- **inception**: A bridge network that connects all the containers.
### Volumes
- **wordpress**: Mounted at /var/www/html for WordPress data.
- **mariadb**: Mounted at /var/lib/mysql for MariaDB data.
- **adminer**: Mounted at /var/www/adminer for Adminer files.

## Environment Variables

The project requires a `.env` file with the following environment variables:

```plaintext
USER
DOMAIN_NAME
CERTS
KEYS
MARIA_DB_NAME
MARIA_USER
MARIA_PASSWORD
MARIA_ROOT_PASSWORD
WP_TITLE
WP_USER
WP_PASSWORD
WP_EMAIL
WP_ROOT_USER
WP_ROOT_PASSWORD
WP_ROOT_EMAIL
FTP_USER
FTP_PASSWORD
```

## How to Run the Project
1. Clone the repository and navigate to the project directory.
2. Create a .env file with the required environment variables and put it in **srcs** folder.
3. Build and start the containers using Makefile:
    ```bash
    make
    ```

4. Access the services via the following URLs:
    - **WordPress** : https://`DOMAIN_NAME`
    - **Hugo** : https://`DOMAIN_NAME`/portfolio
    - **Adminer** : https://`DOMAIN_NAME`/adminer
    - **Grafana** : https://`DOMAIN_NAME`/grafana


### Adminer Configuration

To connect Adminer to your MariaDB database, use the following settings:

1. **System**: `MySQL`
2. **Server**:`MARIA_DB_NAME`
3. **Username**: `MARIA_USER`
4. **Password**: `MARIA_PASSWORD`
5. **Database**: `MARIA_DB_NAME`

All values for `Server`, `Username`, `Password`, and `Database` are sourced from the environment variables defined in your `.env` file.

### Grafana Configuration

When you first access Grafana, you will be prompted to log in:

1. **Login**:
   - **Username**: `admin`
   - **Password**: `admin` (You can change this after logging in for the first time)

2. **Add a Data Source**:
   - Click on **"Add your first data source"** or navigate to **"Data Sources"** from the side menu.
   - Select **"Prometheus"** as the data source type.
   - In the **Connection** settings, set the **URL** to: `http://prometheus:9090`
   - Click on **"Save & Test"** to verify the connection.

3. **Import a Dashboard**:
   - Go back to the Grafana home page and click on **"Create your first dashboard"**.
   - Click on **"Find and Import Dashboard"**.
   - Enter the dashboard ID **`1860`** (Node Exporter Full) and click **"Load"**.
   - In the next step, select **Prometheus** as the data source and click **"Import"**.

4. **View the Dashboard**:
   - You now have a pre-configured dashboard that displays host-level metrics such as CPU usage, memory utilization, disk usage, and more.

The imported dashboard provides detailed insights into the performance of your host machine using metrics collected by the **Node Exporter** and visualized by Grafana.

### Local Testing

If you want to run the project locally on your PC, set `DOMAIN_NAME=localhost` in your `.env` file.

> **Note**: If you use a different domain name, you will need to update your local `/etc/hosts` file to map the domain to `127.0.0.1`. This file requires **sudo permissions** to edit. For example:

```plaintext
127.0.0.1   your-domain-name
```
