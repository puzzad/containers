# Generated from https://github.com/puzzad/containers/postgrest/Dockerfile.gotpl
# BOM: {"github:postgrest/postgrest":"v10.2.0.20230203","image:ghcr.io/greboid/dockerfiles/alpine":"c14b3f57ce420de5e95142c7c8932716108789e552759b355e94df81159f4bbf"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:c14b3f57ce420de5e95142c7c8932716108789e552759b355e94df81159f4bbf as build

ARG TAG=v10.2.0.20230203

RUN set -eux; \
    apk add xz; \
    wget https://github.com/PostgREST/postgrest/releases/download/$TAG/postgrest-$TAG-linux-static-x64.tar.xz; \
    tar xf postgrest-$TAG-linux-static-x64.tar.xz;

FROM scratch
COPY --from=build /postgrest /
ENTRYPOINT ["/postgrest"]
