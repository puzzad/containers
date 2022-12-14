# Generated from https://github.com/puzzad/containers/gotrue/Dockerfile.gotpl
# BOM: {"github:puzzad/gotrue":"v1.1"}

FROM ghcr.io/greboid/dockerfiles/golang:latest as build

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

