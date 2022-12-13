# Generated from https://github.com/puzzad/containers/postgrest/Dockerfile.gotpl
# BOM: {"github:postgrest/postgrest":"v10.1.1.20221212"}

FROM ghcr.io/greboid/dockerfiles/alpine:latest as build

ARG TAG=v10.1.1.20221212

RUN set -eux; \
    apk add xz; \
    wget https://github.com/PostgREST/postgrest/releases/download/$TAG/postgrest-$TAG-linux-static-x64.tar.xz; \
    tar xf postgrest-$TAG-linux-static-x64.tar.xz;

FROM scratch
COPY --from=build /postgrest /
ENTRYPOINT ["/postgrest"]
