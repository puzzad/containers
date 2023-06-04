# Generated from https://github.com/puzzad/containers/kong/Dockerfile.gotpl
# BOM: {"github:kong/docker-kong":"3.3.0","github:kong/kong":"3.3.0","image:ghcr.io/greboid/dockerfiles/alpine":"0b5d59073fe11b8be58f75b47d7103dc11fa119b8173fbff47c4a00c4d9b89e6"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:0b5d59073fe11b8be58f75b47d7103dc11fa119b8173fbff47c4a00c4d9b89e6

ARG KONG_VERSION=3.3.0
ENV KONG_VERSION $KONG_VERSION
ENV TAG=3.3.0

RUN set -ex; \
     apk add bash curl ca-certificates git; \
     curl -fL "https://download.konghq.com/gateway-${KONG_VERSION%%.*}.x-alpine/kong-${KONG_VERSION}.amd64.apk.tar.gz" -o /tmp/kong.apk.tar.gz; \
     git -c advice.detachedHead=false clone --depth=1 -b $TAG --single-branch https://github.com/Kong/docker-kong /git; \
     apk add --no-cache --virtual .build-deps tar gzip; \
     tar -C / -xzf /tmp/kong.apk.tar.gz; \
     apk add --no-cache libstdc++ libgcc openssl pcre perl tzdata libcap zlib zlib-dev bash; \
     adduser -S kong; \
     addgroup -S kong; \
     mkdir -p "/usr/local/kong"; \
     chown -R kong:0 /usr/local/kong; \
     chown kong:0 /usr/local/bin/kong; \
     chmod -R g=u /usr/local/kong; \
     rm -rf /tmp/kong.tar.gz; \
     ln -s /usr/local/openresty/luajit/bin/luajit /usr/local/bin/luajit; \
     ln -s /usr/local/openresty/luajit/bin/luajit /usr/local/bin/lua; \
     ln -s /usr/local/openresty/nginx/sbin/nginx /usr/local/bin/nginx; \
     apk del .build-deps; \
     ls /git; \
     cp /git/docker-entrypoint.sh /docker-entrypoint.sh

USER kong

ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 8000 8443 8001 8444

STOPSIGNAL SIGQUIT

HEALTHCHECK --interval=10s --timeout=10s --retries=10 CMD kong health

CMD ["kong", "docker-start"]
