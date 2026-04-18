# Online MTA-STS testing tool

This tool verifies whether a give host correctly implements the new in-development <a href="https://github.com/mrisher/smtp-sts">MTA-STS standard</a> for downgrade-resistant secure email. It is very new and not very well tested so don't rely on it's result too much.

License: BSD 2-clause license (see LICENSE.txt).


## Requirements

* Docker & Docker Compose V2
* SSH/Terminal access (able to install commands/functions if non-existent)
* Defaultly the DNS servers from QUAD9 are configured in the check.py script.
* Outgoing connections via SMTP (port 25/tcp) are required.


## Ports

- 80


## Environment variables

There are currently no variables.


## Usage
The simplest way to run the container is the following commands.

1. This script will install docker and containerd:
  ```bash
  curl https://raw.githubusercontent.com/wydler/mta-sts-docker/master/docker/misc/02-docker.io-installation.sh | bash
  ```
2. For IPv6 support, edit the Docker daemon configuration file, located at `/etc/docker/daemon.json`. Configure the following parameters and run `systemctl restart docker.service` to restart docker:
  ```bash
  {
    "experimental": true,
    "ip6tables": true
  }
  ```
3. Create a `docker-compose.yml`:
```yml
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

4. Starting application with `docker compose up -d`.
5. Don't forget to test, that the applcation works sucessully (e.g. http://IP-Addresse or FQDN/).