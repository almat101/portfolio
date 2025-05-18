---
title: "Virtual Machine"
description: "Setting up a new VM for inception project"
date: "2024-11-01"
draft: false
tags: ["docker", "VM", "inception"]
showToc: false
weight: 202
cover:
    image: "/logo/vm.png"
---

# Setting Up the VM

After installing a new VM with a Debian image, follow these steps to set up your environment for the Inception project. This guide includes installing Docker, Docker Compose, Git, and Vim.

## Step-by-Step Guide

### 1. Update and Upgrade the System
First, update and upgrade the package list to ensure you have the latest packages.
```bash
sudo apt-get update && sudo apt-get upgrade -y
```

### 2. Install Git and Vim
Then, install Git for downloading inception and vim modify files
```bash
sudo apt install -y git vim
```

### 3. Install Docker

+ Set up docker's apt repository:
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
+ Install docker packages
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

+ Verify the installation by running the hello-world image:
```bash
sudo docker run hello-world
```
