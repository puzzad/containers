# Generated from https://github.com/puzzad/containers/gotrue/Dockerfile.gotpl
# BOM: {"github:puzzad/gotrue":"v1.4","image:ghcr.io/greboid/dockerfiles/golang":"ef186f4f1646a6757769120bf4e33484b3c4d852317839f29ba1df007af07abc"}

FROM ghcr.io/greboid/dockerfiles/golang@sha256:ef186f4f1646a6757769120bf4e33484b3c4d852317839f29ba1df007af07abc as build

ARG TAG=v1.4
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
COPY --from=build /go/src/github.com/netlify/gotrue/templates /usr/local/etc/gotrue/templates/

ENV GOTRUE_DB_MIGRATIONS_PATH /usr/local/etc/gotrue/migrations
ENV TEMPLATE_DIR /usr/local/etc/gotrue/templates

USER netlify
CMD ["gotrue"]

