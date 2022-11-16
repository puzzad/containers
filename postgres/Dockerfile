# Generated from https://github.com/puzzad/containers/postgres/Dockerfile.gotpl
# BOM: {"regexurl:supabase-postgres":"14.1.0.92"}

FROM ghcr.io/greboid/dockerfiles/golang:latest as go
RUN go install github.com/tianon/gosu@latest

FROM ghcr.io/greboid/dockerfiles/alpine:latest as pgbase

#Get Various versions
ARG TAG=14.1.0.92

#Install dependencies
RUN set -eux; \
    MAJORVERSION=$(echo $TAG | cut -d. -f1); \
    apk add build-base make automake autoconf git libpq-dev postgresql${MAJORVERSION}-dev curl-dev libsodium-dev;

#Install pgaudit
RUN set -eux; \
    git clone https://github.com/pgaudit/pgaudit.git /src/audit; \
    cd /src/audit; \
    make install USE_PGXS=1 PG_CONFIG=/usr/bin/pg_config;

#Install pg_jwt
RUN set -eux; \
    git clone https://github.com/michelp/pgjwt /src/jwt; \
    cd /src/jwt; \
    make install;

#Install pg_http
RUN set -eux; \
    git clone https://github.com/pramsey/pgsql-http.git /src/http; \
    cd /src/http; \
    make install;

#Install wal2json
RUN set -eux; \
    git clone https://github.com/eulerto/wal2json.git /src/wal2json; \
    cd /src/wal2json; \
    make install;

#Install pg_net
RUN set -eux; \
    git clone https://github.com/supabase/pg_net.git /src/net; \
    cd /src/net; \
    make install;

#Install pg_hashids
RUN set -eux; \
    git clone https://github.com/iCyberon/pg_hashids.git /src/hashids; \
    cd /src/hashids; \
    USE_PGXS=1 make install;

#Install pg_sodium
RUN set -eux; \
    git clone https://github.com/michelp/pgsodium.git /src/sodium; \
    cd /src/sodium; \
    make install;

#Install pg_cron
RUN set -eux;  \
    git clone https://github.com/citusdata/pg_cron.git /src/cron; \
    cd /src/cron; \
    make install;

#Intall pg_stat_monitor
RUN set -eux; \
    git clone https://github.com/percona/pg_stat_monitor.git /src/stat; \
    cd /src/stat; \
    make USE_PGXS=1 install

#Get postgres for entrypoint
RUN set -eux; \
    git clone https://github.com/docker-library/postgres /src/postgres;

#Get supabase postgres repo for configs
RUN set -eux; \
    git clone https://github.com/supabase/postgres.git /src/supabase; \
    echo "# IPv6 external connections" >> /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2; \
    echo "host  all  all  ::/0          scram-sha-256" >> /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2; \
    sed -i "s/shared_preload_libraries.*/shared_preload_libraries = 'pg_stat_statements, pg_stat_monitor, pgaudit, pg_cron, pg_net, pgsodium'/" /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2; \
    cp /src/supabase/ansible/files/pgsodium_getkey_urandom.sh.j2 /usr/share/postgresql14/extension/pgsodium_getkey; 

FROM ghcr.io/greboid/dockerfiles/alpine:latest

ENV LANGUAGE en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ENV PGDATA /var/lib/postgresql/data

ARG TAG=14.1.0.92

COPY --from=go /go/bin/gosu /usr/bin/gosu
COPY --from=pgbase /usr/lib/postgresql14/ /usr/lib/postgresql14/
COPY --from=pgbase /usr/share/postgresql14/extension /usr/share/postgresql14/extension
COPY --from=pgbase /src/postgres/docker-entrypoint.sh /docker-entrypoint.sh
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2 /etc/postgresql/postgresql.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2 /etc/postgresql/pg_hba.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/pg_ident.conf.j2 /etc/postgresql/pg_ident.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/postgresql-stdout-log.conf /etc/postgresql/logging.conf
COPY --from=pgbase /src/supabase/migrations/db/ /docker-entrypoint-initdb.d/

RUN set -eux; \
    MAJORVERSION=$(echo $TAG | cut -d. -f1); \
    apk add \
	postgresql${MAJORVERSION} \
        postgresql${MAJORVERSION}-contrib \
        bash \
	libcurl \
	libsodium; \
    mkdir -p "$PGDATA"; \
    chown -R postgres:postgres "$PGDATA"; \
    chmod 777 "$PGDATA"

VOLUME /var/lib/postgresql/data
STOPSIGNAL SIGINT
EXPOSE 5432
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres", "-c", "config_file=/etc/postgresql/postgresql.conf"]
