# Generated from https://github.com/puzzad/containers/gotrue/Dockerfile.gotpl
# BOM: {"github:puzzad/gotrue":"v1.1","image:ghcr.io/greboid/dockerfiles/golang":"62336f81c6c2c3ecdf35de9cf62c7ad3e84cebad54841ad004c734da9ca56bbe"}

FROM ghcr.io/greboid/dockerfiles/golang@sha256:62336f81c6c2c3ecdf35de9cf62c7ad3e84cebad54841ad004c734da9ca56bbe as build

ARG TAG=v1.1
RUN apk add build-base
RUN git -c advice.detachedHead=false clone --depth=1 -b $TAG --single-branch https://github.com/puzzad/gotrue /go/src/github.com/netlify/gotrue;
WORKDIR /go/src/github.com/netlify/gotrue
RUN make deps
RUN make build

FROM ghcr.io/greboid/dockerfiles/alpine:latest
RUN adduser -D -u 1000 netlify

RUN apk add --no-cache ca-certificates
COPY --from=build /go/src/github.com/netlify/gotrue/gotrue /usr/local/bin/gotrue
COPY --from=build /go/src/github.com/netlify/gotrue/migrations /usr/local/etc/gotrue/migrations/

ENV GOTRUE_DB_MIGRATIONS_PATH /usr/local/etc/gotrue/migrations

USER netlify
CMD ["gotrue"]

