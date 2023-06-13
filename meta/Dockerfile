# Generated from https://github.com/puzzad/containers/meta/Dockerfile.gotpl
# BOM: {"github:supabase/postgres-meta":"v0.66.3","image:ghcr.io/greboid/dockerfiles/alpine":"0b5d59073fe11b8be58f75b47d7103dc11fa119b8173fbff47c4a00c4d9b89e6"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:0b5d59073fe11b8be58f75b47d7103dc11fa119b8173fbff47c4a00c4d9b89e6 as app

ARG TAG=v0.66.3

RUN set -eux; \
    apk add npm git python3 g++ make bash; \
    git -c advice.detachedHead=false clone --depth=1 -b $TAG --single-branch https://github.com/supabase/postgres-meta.git /app; \
    mkdir /dep; \
    cd /dep; \
    cp /app/package* .; \
    npm clean-install; \
    cd /app; \
    cp -r /dep/node_modules .; \
    npm run build;

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:0b5d59073fe11b8be58f75b47d7103dc11fa119b8173fbff47c4a00c4d9b89e6

RUN apk add nodejs

WORKDIR /app
COPY --from=app /app/node_modules node_modules
COPY --from=app /app/dist dist
COPY --from=app /app/package.json ./
COPY --from=app /app/package-lock.json ./

RUN ls -lR /app

ENV PG_META_PORT=8080
EXPOSE 8080
CMD ["node", "dist/server/app.js"]
