# Generated from https://github.com/puzzad/containers/gotrue/Dockerfile.gotpl
# BOM: {"github:puzzad/gotrue":"v1.4","image:ghcr.io/greboid/dockerfiles/golang":"cd1d3066f49fd7bf90666c6c47950c4cefebde0b36b5e403b67e6fda145a52bb"}

FROM ghcr.io/greboid/dockerfiles/golang@sha256:cd1d3066f49fd7bf90666c6c47950c4cefebde0b36b5e403b67e6fda145a52bb as build

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

