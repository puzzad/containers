# Generated from https://github.com/puzzad/containers/postgrest/Dockerfile.gotpl
# BOM: {"github:postgrest/postgrest":"v10.2.0.20230209","image:ghcr.io/greboid/dockerfiles/alpine":"e3385453363598236b7e826530c55b70d4a1a3e53931d4e7b675d47c4c5c8f8d"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:e3385453363598236b7e826530c55b70d4a1a3e53931d4e7b675d47c4c5c8f8d as build

ARG TAG=v10.2.0.20230209

RUN set -eux; \
    apk add xz; \
    wget https://github.com/PostgREST/postgrest/releases/download/$TAG/postgrest-$TAG-linux-static-x64.tar.xz; \
    tar xf postgrest-$TAG-linux-static-x64.tar.xz;

FROM scratch
COPY --from=build /postgrest /
ENTRYPOINT ["/postgrest"]
