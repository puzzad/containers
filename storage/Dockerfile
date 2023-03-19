# Generated from https://github.com/puzzad/containers/storage/Dockerfile.gotpl
# BOM: {"github:supabase/storage-api":"v0.30.2","image:ghcr.io/greboid/dockerfiles/alpine":"e3385453363598236b7e826530c55b70d4a1a3e53931d4e7b675d47c4c5c8f8d"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:e3385453363598236b7e826530c55b70d4a1a3e53931d4e7b675d47c4c5c8f8d as npm

RUN set -eux; \
    apk add npm git python3 g++ make;

FROM npm as checkout

ARG TAG=v0.30.2

RUN set -eux; \
    git -c advice.detachedHead=false clone --depth=1 -b $TAG --single-branch https://github.com/supabase/storage-api.git /app;

FROM npm as deps

COPY --from=checkout /app/package.json /app/package.json
COPY --from=checkout /app/package-lock.json /app/package-lock.json

RUN set -eux; \
    apk add g++ make python3 git; \
    cd /app; \
    npm ci --omit=dev;

FROM npm as app

COPY --from=checkout /app /app
RUN set -eux; \
    cd /app; \
    npm ci; \
    npm run build;

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:e3385453363598236b7e826530c55b70d4a1a3e53931d4e7b675d47c4c5c8f8d

COPY --from=checkout /app/migrations /migrations
COPY --from=checkout /app/ecosystem.config.js /ecosystem.config.js
COPY --from=checkout /app/docker-entrypoint.sh /docker-entrypoint.sh
COPY --from=deps /app/node_modules /node_modules
COPY --from=app /app/dist /dist

RUN set -eux; \
    apk add bash npm; \
    npm install -g pm2

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["pm2-runtime", "/ecosystem.config.js"]
