############################
# Stage 1: Runtime
############################
FROM ubuntu:24.04@sha256:561618e2c15bf2397621dd04f96926663a3b5616c189cf7e38db7e82f5c538ea


# Setze Arbeitsverzeichnis für temporäre Dateien
WORKDIR /app

############################
# Metadata
############################
LABEL maintainer="Daniel Wydler" \
      org.opencontainers.image.authors="Daniel Wydler" \
      org.opencontainers.image.description="Online tool for MTA-STS checking." \
      org.opencontainers.image.documentation="https://github.com/wydler/mta-sts-docker/blob/master/README.md" \
      org.opencontainers.image.source="https://github.com/wydler/mta-sts-docker" \
      org.opencontainers.image.title="wydler/mta-sts-uwsgi" \
      org.opencontainers.image.url="https://hub.docker.com/r/wydler/mta-sts-uwsgi"


#############################################
# Runtime dependencies
#############################################
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    uwsgi \
    uwsgi-plugin-python3 \
    python3-dnspython \
    net-tools \
    iputils-ping \
    python3-pip \
    python3-cryptography \
    python3-flask \
    python3-flask-limiter \
    python3.12-venv \
    && rm -rf /var/lib/apt/lists/*


#############################################
# Python dependencies (via pip)
#############################################
COPY docker/ubuntu/requirements.txt /app
RUN python3 -m venv /app/venv \
    && /app/venv/bin/pip install --no-cache-dir -r /app/requirements.txt \
    && rm -r /app/requirements.txt

ENV PATH="/app/venv/bin:$PATH"


#############################################
# uwsgi configuration
# Kopiert die angepasste uwsgi Konfigurationen
#############################################
COPY rootfs/uwsgi/etc/uwsgi/ /etc/uwsgi/
RUN chmod 755 /etc/uwsgi/vassals


#############################################
# Web Application Files
# MTA-STS Web Content wird ins Webroot kopiert
#############################################
RUN mkdir -p /var/www/html/mta-sts
COPY rootfs/mta-sts /var/www/html/mta-sts/


#############################################
# Permissions
# Stellt sicher, dass uwsgi Prozess Zugriff
# auf die Web-Dateien hat
#############################################
RUN chown -R www-data:www-data /var/www/html/mta-sts


############################
# CMD
############################
CMD ["uwsgi", "--ini", "/etc/uwsgi/vassals/mta-sts.ini"]
