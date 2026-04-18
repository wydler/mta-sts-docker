############################
# Stage 1: Runtime
############################
FROM nginx:1.30.0@sha256:0c3a604353135ac13f3a991e2a98c9958af7b8c0b7251ce1590e0c5ff5e3692c


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
  CMD wget -qO- http://localhost/ || exit 1