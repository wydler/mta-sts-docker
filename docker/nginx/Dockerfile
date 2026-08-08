############################
# Stage 1: Runtime
############################
FROM nginx:1.31.3@sha256:8541484afbc9c8a5a8a99b379568ebbc957f658583ec9448fc43104229c03cf8


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