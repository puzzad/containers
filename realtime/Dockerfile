# Generated from https://github.com/puzzad/containers/realtime/Dockerfile.gotpl
# BOM: {"image:ghcr.io/greboid/dockerfiles/alpine":"c14b3f57ce420de5e95142c7c8932716108789e552759b355e94df81159f4bbf"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:c14b3f57ce420de5e95142c7c8932716108789e552759b355e94df81159f4bbf as build

ARG TAG=v1.0.0

ENV MIX_ENV=prod \
    DB_SSL=false

RUN set -eux; \
    apk add elixir git; \
    mix local.hex --force && mix local.rebar --force; \
    git -c advice.detachedHead=false clone --depth=1 -b $TAG --single-branch https://github.com/supabase/realtime.git /app; \
    cd /app;
RUN set -eux; \
    cd /app/server; \
    ls; \
    mix deps.get; \
    mix deps.compile; \
    mix release

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:c14b3f57ce420de5e95142c7c8932716108789e552759b355e94df81159f4bbf

RUN apk add openssl bash libstdc++ libgcc
WORKDIR /app
COPY --from=build /app/server/_build /app
COPY --from=build /app/docker-entrypoint.sh /app/docker-entrypoint.sh
ENTRYPOINT ["/app/docker-entrypoint.sh"]
CMD ["/app/prod/rel/realtime/bin/realtime", "start"]
