# Online MTA-STS testing tool

This tool verifies whether a give host correctly implements the new in-development MTA-STS standard for downgrade-resistant secure email. It is very new and not very well tested so don't rely on it's result too much.

[![Build and Publish Docker Image](https://github.com/wydler/mta-sts-docker/actions/workflows/build.docker.images.yml/badge.svg)](https://github.com/wydler/mta-sts-docker/blob/master/.github/workflows/build.docker.images.yml)

## 📦 Available Tags

**Choose the right version for your environment:**

- **`latest`**: Latest stable build - recommended for production
- **`2.0.0`**: Specific stable version - recommended for production
- **`master`**: Latest development build - recommended for testing

Example: `wydler/mta-sts-uwsgi:2.0.0`

## 🔄 Overview

This container runs a Web UI for testing the new MTA-STS standard for downgrade-resistant secure email. It helps to check if all necessary DNS entries and configurations are correct.

## ✨ Features

- Lightweight & secure image (no root process)
- Modern Web UI
- Based on Ubuntu Linux
- With Nginx

## 🚀 Quick Start

```yaml
---
services:
  web:
    image: "wydler/mta-sts-web:latest"
    container_name: "mta-sts_webserver"
    restart: "unless-stopped"
    ports:
      - "80:80"
    volumes:
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
    links:
      - app


  app:
    image: "wydler/mta-sts-uwsgi:latest"
    container_name: "mta-sts_application"
    restart: "unless-stopped"
    user: "33:33"
    volumes:
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
      - "/var/run/docker.sock:/var/run/docker.sock:ro"


networks:
  default:
    enable_ipv6: true
    name: wydler-mta-sts_default
```

## ⚙️ Environment Variables

There are currently no variables.

## 🔗 Links

- [GitHub Repository](https://github.com/wydler/mta-sts-docker)
- [Full Documentation](https://github.com/wydler/mta-sts-docker/blob/master/README.md)

## 📄 License

This project is licensed under the BSD 2-clause license.