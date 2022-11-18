# Generated from https://github.com/puzzad/containers/meta/Dockerfile.gotpl
# BOM: {"github:supabase/postgres-meta":"v0.50.2"}

FROM ghcr.io/greboid/dockerfiles/alpine:latest as app

ARG TAG=v0.50.2

RUN set -eux; \
    apk add npm git python3 g++ make bash; \
    git -c advice.detachedHead=false clone --depth=1 -b $TAG --single-branch https://github.com/supabase/postgres-meta.git /app; \
    mkdir /dep; \
    cd /dep; \
    cp /app/package* .; \
    npm clean-install; \
    cd /app; \
    cp -r /dep/node_modules .; \
    npm run build:server;

FROM ghcr.io/greboid/dockerfiles/alpine:latest

RUN apk add npm

WORKDIR /usr/src/app
COPY --from=app /app/node_modules node_modules
COPY --from=app /app/bin bin
COPY --from=app /app/package.json ./
COPY --from=app /app/package-lock.json ./

ENV PG_META_PORT=8080
EXPOSE 8080
CMD ["npm", "run", "start"]