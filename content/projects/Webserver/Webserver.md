---
title: "Webserver"
description: "An HTTP server developed from scratch"
date: "2024-09-26"
draft: false
tags: ["HTTP", "C++", "GET", "POST", "DELETE", "nginx"]
showToc: false
weight: 201
cover:
    image: "/logo/webserver.png"
---
### 🔗 [Subject](https://github.com/mlongo03/WebServ_42/blob/main/var/www/WebServer.pdf)

## Description

In this project, done togheter with my peer Manuele Longo we developed a fully functional HTTP web server built in C++98, supporting essential web server features like GET, POST, and DELETE requests, CGI handling, and more.

## Purpose

The purpose of this project is to create a robust and efficient web server that can handle various HTTP requests, serve static and dynamic content, and manage multiple virtual hosts. The server is designed to be lightweight, performant, and compliant with HTTP/1.1 standards.

## Features

+ HTTP/1.1 Compliance: Supports basic HTTP request methods (GET, POST, DELETE).
+ Virtual Hosts: Multiple server configurations on the same IP and port, distinguished by the hostname.
+ CGI Support: Handles CGI scripts for dynamic content generation (e.g., PHP, Python).
+ Static File Serving: Serves static files from specified directories.
+ Error Pages: Custom error pages for 404, 403, 500 errors, etc.
+ Chunked Transfer Encoding: Properly handles chunked data in requests.
+ Multithreaded/Multiprocess: Can handle concurrent connections using epoll (or other async mechanisms).
+ Logging: Provides basic logs for connections and errors.
+ Stress-Tested: Battle-tested with tools like siege for stability and performance.

For more detailed information [click here](https://github.com/mlongo03/WebServ_42)
