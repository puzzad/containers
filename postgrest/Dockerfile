# Generated from https://github.com/puzzad/containers/postgrest/Dockerfile.gotpl
# BOM: {"github:postgrest/postgrest":"v11.0.1","image:ghcr.io/greboid/dockerfiles/alpine":"0b5d59073fe11b8be58f75b47d7103dc11fa119b8173fbff47c4a00c4d9b89e6"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:0b5d59073fe11b8be58f75b47d7103dc11fa119b8173fbff47c4a00c4d9b89e6 as build

ARG TAG=v11.0.1

RUN set -eux; \
    apk add xz; \
    wget https://github.com/PostgREST/postgrest/releases/download/$TAG/postgrest-$TAG-linux-static-x64.tar.xz; \
    tar xf postgrest-$TAG-linux-static-x64.tar.xz;

FROM scratch
COPY --from=build /postgrest /
ENTRYPOINT ["/postgrest"]
