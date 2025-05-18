---
title: "Docker"
description: "What is Docker?"
date: "2024-11-03"
draft: false
tags: ["docker", "image", "container"]
showToc: false
weight: 202
cover:
    image: "/logo/docker.webp"
---

## What is Docker?

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization. Containers allow developers to package an application with all its dependencies into a standardized unit for software development.

## Key Concepts

### Containers
Containers are lightweight, portable, and self-sufficient units that include everything needed to run a piece of software, including the code, runtime, libraries, and system tools. They are isolated from each other and the host system, ensuring consistent behavior across different environments.

### Images
Docker images are read-only templates used to create containers. An image includes the application code, libraries, dependencies, and other files needed to run the application. Images are built from a Dockerfile, which contains a series of instructions for creating the image.

### Dockerfile
A Dockerfile is a text file that contains a series of instructions for building a Docker image. It specifies the base image, application code, dependencies, and other configurations needed to create the image.

### Docker Hub
Docker Hub is a cloud-based registry service that allows you to store, share, and manage Docker images. It provides a centralized location for finding and distributing container images.

### Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file to configure the application's services, networks, and volumes, allowing you to manage complex applications with ease.

## Benefits of Docker

### Portability
Docker containers can run on any system that supports Docker, ensuring consistent behavior across different environments, such as development, testing, and production.

### Isolation
Containers provide process and filesystem isolation, ensuring that applications run in their own environments without interfering with each other or the host system.

### Scalability
Docker makes it easy to scale applications horizontally by adding or removing containers as needed. This allows for efficient resource utilization and improved application performance.

### Efficiency
Containers are lightweight and share the host system's kernel, making them more efficient than traditional virtual machines. This results in faster startup times and reduced resource consumption.

### Version Control
Docker images are versioned, allowing you to track changes and roll back to previous versions if needed. This makes it easy to manage application updates and ensure consistency across different environments.

## How Docker Works

1. **Build**: Create a Dockerfile with the necessary instructions to build the image. Use the `docker build` command to create the image from the Dockerfile.
2. **Ship**: Push the image to Docker Hub or another registry using the `docker push` command. This makes the image available for others to use.
3. **Run**: Use the `docker run` command to create and start a container from the image. The container runs the application in an isolated environment.

## Example Dockerfile

Here is an example of a simple Dockerfile for an ubuntu-based application:

```dockerfile
# Use the official Ubuntu image as the base image
FROM ubuntu:latest

# Update and upgrade the package list
RUN apt-get update && apt-get upgrade -y

# Install necessary packages
RUN apt-get install -y vim

# Start the application
CMD ["bash"]
```
In this case we install only vim, but you can install and configure every package you want, like nginx, wordpress, apache, mariadb etc
