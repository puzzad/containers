# Generated from https://github.com/puzzad/containers/realtime/Dockerfile.gotpl
# BOM: {"github:supabase/realtime":"v2.1.0"}

FROM ghcr.io/greboid/dockerfiles/alpine:latest as build

ARG TAG=v2.1.0

ENV MIX_ENV=prod \
    DB_SSL=false

RUN set -eux; \
    apk add elixir git npm; \
    mix local.hex --force && mix local.rebar --force; \
    git -c advice.detachedHead=false clone --depth=1 -b $TAG --single-branch https://github.com/supabase/realtime.git /app; \
    cd /app;
RUN set -eux; \
    cd /app; \
    mix deps.get --only $MIX_ENV; \
    mix deps.compile; \
    cd assets; \
    npm install; \
    cd ..; \
    mix assets.deploy; \
    mix compile; \
    mix release

FROM ghcr.io/greboid/dockerfiles/alpine:latest

RUN apk add openssl bash libstdc++ libgcc
WORKDIR /app
COPY --from=build /app/_build /app
CMD ["/app/_build/prod/rel/realtime", "start"]
