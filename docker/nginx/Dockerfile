############################
# Stage 1: Runtime
############################
FROM nginx:1.30.0@sha256:50e2952b555d082947d1f70b190a6da1e731ba879116e238f4d1d5ade6b49b40


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