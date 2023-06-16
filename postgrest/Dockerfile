# Generated from https://github.com/puzzad/containers/postgrest/Dockerfile.gotpl
# BOM: {"github:postgrest/postgrest":"v11.1.0","image:ghcr.io/greboid/dockerfiles/alpine":"bb09baaefa0022cb709bfb42e8852359b208eb2a6baa0beff38eaa3a610141a7"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:bb09baaefa0022cb709bfb42e8852359b208eb2a6baa0beff38eaa3a610141a7 as build

ARG TAG=v11.1.0

RUN set -eux; \
    apk add xz; \
    wget https://github.com/PostgREST/postgrest/releases/download/$TAG/postgrest-$TAG-linux-static-x64.tar.xz; \
    tar xf postgrest-$TAG-linux-static-x64.tar.xz;

FROM scratch
COPY --from=build /postgrest /
ENTRYPOINT ["/postgrest"]
