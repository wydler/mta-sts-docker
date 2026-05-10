############################
# Stage 1: Runtime
############################
FROM nginx:1.30.0@sha256:55d1fb0944aafb8534b253e486c6cf57d3e0c785c4f6a7b3e9f02c5a080fb2ce


############################
# Metadata
############################
LABEL maintainer="Daniel Wydler" \
      org.opencontainers.image.authors="Daniel Wydler" \
      org.opencontainers.image.description="Online tool for MTA-STS checking." \
      org.opencontainers.image.documentation="https://github.com/wydler/mta-sts-docker/blob/master/README.md" \
      org.opencontainers.image.source="https://github.com/wydler/mta-sts-docker" \
      org.opencontainers.image.title="wydler/mta-sts-web" \
      org.opencontainers.image.url="https://hub.docker.com/r/wydler/mta-sts-web"


#############################################
# nginx configuration
# Kopiert die angepasste nginx Konfiguration
# direkt in das Standard nginx config Verzeichnis
#############################################
COPY rootfs/nginx/etc/nginx/ /etc/nginx/


#############################################
# Web Application Files
# MTA-STS Web Content wird ins Webroot kopiert
#############################################
RUN mkdir -p /var/www/html/mta-sts
COPY rootfs/mta-sts /var/www/html/mta-sts/


#############################################
# Permissions
# Stellt sicher, dass nginx Prozess Zugriff
# auf die Web-Dateien hat
#############################################
RUN chown -R nginx:nginx /var/www/html/mta-sts


#############################################
# Healthcheck (optional aber empfohlen)
# Prüft regelmäßig, ob der Webserver erreichbar ist
#############################################
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1