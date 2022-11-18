# Generated from https://github.com/puzzad/containers/postgres/Dockerfile.gotpl
# BOM: {"github:citusdata/pg_cron":"v1.4.2","github:iCyberon/pg_hashids":"v1.2.1","github:michelp/pgsodium":"v3.0.6","github:percona/pg_stat_monitor":"1.1.1","github:pgaudit/pgaudit":"1.7.0","github:pramsey/pgsql-http":"v1.5.0","github:supabase/pg_net":"v0.6.1","github:theory/pgtap/":"v1.2.0","regexurl:supabase-postgres":"14.1.0.94"}

FROM ghcr.io/greboid/dockerfiles/golang:latest as go
RUN go install github.com/tianon/gosu@latest

FROM ghcr.io/greboid/dockerfiles/alpine:latest as pgbase

#Get Various versions
ARG POSTGRES=14.1.0.94
ARG AUDIT=1.7.0
ARG JWT=9742dab1b2f297ad3811120db7b21451bca2d3c9
ARG HTTP=v1.5.0
ARG WALJSON=wal2json_2_5
ARG NET=v0.6.1
ARG HASHIDS=v1.2.1
ARG SODIUM=v3.0.6
ARG CRON=v1.4.2
ARG STAT=1.1.1
ARG TAP=v1.2.0

#Install dependencies
RUN set -eux; \
    MAJORVERSION=$(echo $POSTGRES | cut -d. -f1); \
    apk add build-base make automake autoconf git libpq-dev postgresql${MAJORVERSION}-dev curl-dev libsodium-dev; \
    git config --global advice.detachedHead false

#Install pgaudit
RUN set -eux; \
    git clone --depth=1 -b $AUDIT --single-branch https://github.com/pgaudit/pgaudit.git /src/audit; \
    cd /src/audit; \
    make install USE_PGXS=1 PG_CONFIG=/usr/bin/pg_config;

#Install pg_jwt
RUN set -eux; \
    git clone --depth=1 -b master --single-branch https://github.com/michelp/pgjwt /src/jwt; \
    cd /src/jwt; \
    git checkout $JWT; \
    make install;

#Install pg_http
RUN set -eux; \
    git clone --depth=1 -b $HTTP --single-branch https://github.com/pramsey/pgsql-http.git /src/http; \
    cd /src/http; \
    make install;

#Install wal2json
RUN set -eux; \
    git clone --depth=1 -b $WALJSON --single-branch https://github.com/eulerto/wal2json.git /src/wal2json; \
    cd /src/wal2json; \
    make install;

#Install pg_net
RUN set -eux; \
    git clone --depth=1 -b $NET --single-branch https://github.com/supabase/pg_net.git /src/net; \
    cd /src/net; \
    make install;

#Install pg_hashids
RUN set -eux; \
    git clone --depth=1 -b $HASHIDS --single-branch https://github.com/iCyberon/pg_hashids.git /src/hashids; \
    cd /src/hashids; \
    USE_PGXS=1 make install;

#Install pg_sodium
RUN set -eux; \
    git clone --depth=1 -b $SODIUM --single-branch https://github.com/michelp/pgsodium.git /src/sodium; \
    cd /src/sodium; \
    make install;

#Install pg_cron
RUN set -eux;  \
    git clone --depth=1 -b $CRON --single-branch https://github.com/citusdata/pg_cron.git /src/cron; \
    cd /src/cron; \
    make install;

#Intall pg_stat_monitor
RUN set -eux; \
    git clone --depth=1 -b $STAT --single-branch https://github.com/percona/pg_stat_monitor.git /src/stat; \
    cd /src/stat; \
    make USE_PGXS=1 install

#Install PGTap
RUN set -eux; \
    git clone --depth=1 -b $TAP --single-branch https://github.com/theory/pgtap.git /src/tap; \
    cd /src/tap; \
    make install

#Get postgres for entrypoint
RUN set -eux; \
    git clone https://github.com/docker-library/postgres /src/postgres;

#Get supabase postgres repo for configs
RUN set -eux; \
    git clone https://github.com/supabase/postgres.git /src/supabase; \
    echo "# IPv6 external connections" >> /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2; \
    echo "host  all  all  ::/0          scram-sha-256" >> /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2; \
    sed -i "s/shared_preload_libraries.*/shared_preload_libraries = 'pg_stat_statements, pg_stat_monitor, pgaudit, pg_cron, pg_net, pgsodium'/" /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2; \
    echo "pgsodium.getkey_script = '/usr/share/postgresql14/extension/pgsodium_getkey'" >> /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2; \
    cp /src/supabase/ansible/files/pgsodium_getkey_urandom.sh.j2 /usr/share/postgresql14/extension/pgsodium_getkey; \
    chmod +x /usr/share/postgresql14/extension/pgsodium_getkey;

FROM ghcr.io/greboid/dockerfiles/alpine:latest

ENV LANGUAGE en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ENV PGDATA /var/lib/postgresql/data

ARG POSTGRES=14.1.0.94

RUN set -eux; \
    MAJORVERSION=$(echo $POSTGRES | cut -d. -f1); \
    apk add \
	postgresql${MAJORVERSION} \
        postgresql${MAJORVERSION}-jit \
        postgresql${MAJORVERSION}-contrib \
        bash \
	libcurl \
        libsodium-dev \
	libsodium; \
    rm -rf /var/cache/apk/*; \
    mkdir -p /etc/postgresql-custom/; \
    chown -R postgres:postgres /etc/postgresql-custom/; \
    chmod 777 /etc/postgresql-custom/; \
    mkdir -p "$PGDATA"; \
    chown -R postgres:postgres "$PGDATA"; \
    chmod 777 "$PGDATA"; \
    mkdir -p /run/postgresql; \
    chmod 777 /run/postgresql;

COPY --from=go /go/bin/gosu /usr/bin/gosu
COPY --from=pgbase /usr/lib/postgresql14/ /usr/lib/postgresql14/
COPY --from=pgbase /usr/share/postgresql14/extension /usr/share/postgresql14/extension
COPY --from=pgbase /src/postgres/docker-entrypoint.sh /docker-entrypoint.sh
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2 /etc/postgresql/postgresql.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2 /etc/postgresql/pg_hba.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/pg_ident.conf.j2 /etc/postgresql/pg_ident.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/postgresql-stdout-log.conf /etc/postgresql/logging.conf
COPY --from=pgbase /src/supabase/migrations/db/ /docker-entrypoint-initdb.d/

RUN /usr/share/postgresql14/extension/pgsodium_getkey

VOLUME /var/lib/postgresql/data
STOPSIGNAL SIGINT
EXPOSE 5432
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres", "-c", "config_file=/etc/postgresql/postgresql.conf"]
