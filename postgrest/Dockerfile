# Generated from https://github.com/puzzad/containers/postgrest/Dockerfile.gotpl
# BOM: {"github:postgrest/postgrest":"v10.2.0.20230407","image:ghcr.io/greboid/dockerfiles/alpine":"e05ad3477f38c022bccca78bf22d39f9355bd07a65f4b7d224324c328a84d51b"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:e05ad3477f38c022bccca78bf22d39f9355bd07a65f4b7d224324c328a84d51b as build

ARG TAG=v10.2.0.20230407

RUN set -eux; \
    apk add xz; \
    wget https://github.com/PostgREST/postgrest/releases/download/$TAG/postgrest-$TAG-linux-static-x64.tar.xz; \
    tar xf postgrest-$TAG-linux-static-x64.tar.xz;

FROM scratch
COPY --from=build /postgrest /
ENTRYPOINT ["/postgrest"]
