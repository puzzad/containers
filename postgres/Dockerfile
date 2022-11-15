# Generated from https://github.com/puzzad/containers/postgres/Dockerfile.gotpl
# BOM: {"regexurl:supabase-postgres":"14.1.0.90"}

FROM ghcr.io/greboid/dockerfiles/golang:latest as go

RUN go install github.com/tianon/gosu@latest

FROM ghcr.io/greboid/dockerfiles/alpine:latest as pgbase
ARG TAG=14.1.0.90
RUN set -eux; \
    MAJORVERSION=$(echo $TAG | cut -d. -f1); \
    apk add build-base make automake autoconf git libpq-dev postgresql${MAJORVERSION}-dev curl-dev libsodium-dev; \
    git clone https://github.com/pgaudit/pgaudit.git /src/audit; \
    cd /src/audit; \
    git checkout REL_${MAJORVERSION}_STABLE; \
    make install USE_PGXS=1 PG_CONFIG=/usr/bin/pg_config; \
    git clone https://github.com/michelp/pgjwt /src/jwt; \
    cd /src/jwt; \
    make install; \
    git clone https://github.com/pramsey/pgsql-http.git /src/http; \
    cd /src/http; \
    make install; \
    git clone https://github.com/eulerto/wal2json.git /src/wal2json; \
    cd /src/wal2json; \
    make install; \
    git clone https://github.com/supabase/pg_net.git /src/net; \
    cd /src/net; \
    make install; \
    git clone https://github.com/iCyberon/pg_hashids.git /src/hashids; \
    cd /src/hashids; \
    USE_PGXS=1 make install; \
    git clone https://github.com/michelp/pgsodium.git /src/sodium; \
    cd /src/sodium; \
    make install; \ 
    git clone https://github.com/citusdata/pg_cron.git /src/cron; \
    cd /src/cron; \
    make install; \
    git clone https://github.com/docker-library/postgres /src/postgres;



FROM ghcr.io/greboid/dockerfiles/alpine:latest

ENV LANGUAGE en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ENV PGDATA /var/lib/postgresql/data

ARG TAG=14.1.0.90

COPY --from=go /go/bin/gosu /usr/bin/gosu
COPY --from=pgbase /usr/lib/postgresql14/ /usr/lib/postgresql14/
COPY --from=pgbase /usr/share/postgresql14/extension /usr/share/postgresql14/extension
COPY --from=pgbase /src/postgres/docker-entrypoint.sh /docker-entrypoint.sh

RUN set -eux; \
    MAJORVERSION=$(echo $TAG | cut -d. -f1); \
    apk add \
	postgresql$MAJORVERSION \
        bash; \
    mkdir -p "$PGDATA"; \
    chown -R postgres:postgres "$PGDATA"; \
    chmod 777 "$PGDATA"

VOLUME /var/lib/postgresql/data
STOPSIGNAL SIGINT
EXPOSE 5432
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres", "-c", "config_file=/etc/postgresql/postgresql.conf"]
