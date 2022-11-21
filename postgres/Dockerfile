# Generated from https://github.com/puzzad/containers/postgres/Dockerfile.gotpl
# BOM: {"github:citusdata/pg_cron":"v1.4.2","github:eradman/pg-safeupdate":"1.4","github:iCyberon/pg_hashids":"v1.2.1","github:michelp/pgsodium":"v3.0.6","github:percona/pg_stat_monitor":"1.1.1","github:pgaudit/pgaudit":"1.7.0","github:pramsey/pgsql-http":"v1.5.0","github:supabase/pg_net":"v0.6.1","github:theory/pgtap/":"v1.2.0","regexurl:supabase-postgres":"14.1.0.95"}

FROM ghcr.io/greboid/dockerfiles/golang:latest as go
RUN go install github.com/tianon/gosu@latest

FROM debian:stable-slim as deb

RUN set -eux; \
     apt update; \
     apt install -y --no-install-recommends --no-install-suggests \
	gpg \
	lsb-release \
	ca-certificates \
	libsodium23 \
        curl; \
    curl -Ss http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/postgres.gpg; \
    echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list; \
    echo "Package: *" > /etc/apt/preferences.d/postgres.pref; \
    echo "Pin: release o=apt.postgresql.org" >> /etc/apt/preferences.d/postgres.pref; \
    echo "Pin-Priority: 500" >> /etc/apt/preferences.d/postgres.pref; \
    rm -rf /var/lib/apt/lists/*; \
    apt update; \
    apt upgrade -y; \
    apt install -y --no-install-recommends --no-install-suggests \
	libelogind0; \
    rm -rf /var/lib/apt/lists/*; \
    rm -rf /var/cache/apt/archives ; \
    rm -rf /usr/share/doc/; \
    rm -rf /usr/share/man/; \
    rm -rf /usr/share/locale/;

FROM deb as pgbase

#Get Various versions
ARG POSTGRES=14.1.0.95
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
ARG UPDATE=1.4

#Install dependencies
RUN set -eux; \
    MAJORVERSION=$(echo $POSTGRES | cut -d. -f1); \
    apt update; \
    apt install -y --no-install-recommends --no-install-suggests \
	build-essential \
	git \
	libpq-dev \
	postgresql-server-dev-${MAJORVERSION} \
	postgresql-${MAJORVERSION} \
	libkrb5-dev \
	libsodium-dev \
	libcurl4-openssl-dev \
	libtool \
	automake \
	autoconf \
	pkgconf \
	libevent-dev \
	pandoc \
	libreadline-dev \
	bison \
	flex \
	libreadline8 \
	zlib1g \
        coreutils \
	zlib1g-dev; \
    rm -rf /var/lib/apt/lists/*; \
    rm -rf /var/cache/apt/archives ; \
    rm -rf /usr/share/doc/; \
    rm -rf /usr/share/man/; \
    rm -rf /usr/share/locale/; \
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
    make all; \
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

#Install safeupdate
RUN set -eux; \
    git clone --depth=1 -b $UPDATE --single-branch https://github.com/eradman/pg-safeupdate /src/update; \
    cd /src/update; \
    gmake install

#Get postgres for entrypoint
RUN set -eux; \
    git clone https://github.com/docker-library/postgres /src/postgres;

#Get supabase postgres repo for configs
RUN set -eux; \
    git clone https://github.com/supabase/postgres.git /src/supabase; \
    echo "# IPv6 external connections" >> /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2; \
    echo "host  all  all  ::/0          scram-sha-256" >> /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2; \
    sed -i "s/shared_preload_libraries.*/shared_preload_libraries = 'pg_stat_statements, pg_stat_monitor, pgaudit, pg_cron, pg_net, pgsodium'/" /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2; \
    echo "pgsodium.getkey_script = '/usr/share/postgresql/14/extension/pgsodium_getkey'" >> /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2; \
    cp /src/supabase/ansible/files/pgsodium_getkey_urandom.sh.j2 /usr/share/postgresql/14/extension/pgsodium_getkey; \
    chmod +x /usr/share/postgresql/14/extension/pgsodium_getkey; \
    echo "CREATE SCHEMA IF NOT exists extensions; CREATE EXTENSION IF NOT EXISTS pg_stat_statements with schema extensions;" > /src/supabase/migrations/db/migrations/10000000000001_pg_stat_stagements.sql; \
    rm /src/supabase/migrations/db/migrations/10000000000000_demote-postgres.sql; \
    cp /src/supabase/ansible/files/pgbouncer_config/pgbouncer_auth_schema.sql /src/supabase/migrations/db/migrations/10000000000000_pgbouncer.sql; \
    rm /src/supabase/migrations/db/migrations/20220404205710_pg_graphql-on-by-default.sql; \
    rm /src/supabase/migrations/db/migrations/20220613123923_pg_graphql-pg-dump-perms.sql; \
    rm /src/supabase/migrations/db/migrations/20220317095840_pg_graphql.sql;

FROM deb

ENV PGDATA /var/lib/postgresql/data
ENV PATH /usr/lib/postgresql/14/bin:$PATH

ENV LANGUAGE en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ARG POSTGRES=14.1.0.95

RUN set -eux; \
    MAJORVERSION=$(echo $POSTGRES | cut -d. -f1); \
    apt update; \
    apt-get install -y --no-install-recommends  \
	postgresql-${MAJORVERSION} \
        postgresql-contrib-${MAJORVERSION} \
        bash \
	locales \
	libcurl4 \
        libevent-2. \
	libsodium23; \
    rm -rf /var/lib/apt/lists/*; \
    rm -rf /var/cache/apt/archives ; \
    rm -rf /usr/share/doc/; \
    rm -rf /usr/share/man/; \
    rm -rf /usr/share/locale/; \
    apt install -y --no-install-recommends --no-install-suggests \
	libelogind0; \
    mkdir -p /etc/postgresql-custom/; \
    chown -R postgres:postgres /etc/postgresql-custom/; \
    chmod 777 /etc/postgresql-custom/; \
    mkdir -p "$PGDATA"; \
    chown -R postgres:postgres "$PGDATA"; \
    chmod 777 "$PGDATA"; \
    mkdir -p /run/postgresql; \
    chmod 777 /run/postgresql; \
    echo "LC_ALL=en_US.UTF-8" >> /etc/environment; \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen; \
    echo "LANG=en_US.UTF-8" > /etc/locale.conf; \
    update-locale "en_US.UTF-8"; \
    locale-gen --purge "en_US.UTF-8"; \
    dpkg-reconfigure --frontend noninteractive locales

COPY --from=go /go/bin/gosu /usr/bin/gosu
COPY --from=pgbase /usr/lib/postgresql/14/ /usr/lib/postgresql/14/
COPY --from=pgbase /usr/share/postgresql/14/extension /usr/share/postgresql/14/extension
COPY --from=pgbase /src/postgres/docker-entrypoint.sh /docker-entrypoint.sh
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/postgresql.conf.j2 /etc/postgresql/postgresql.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/pg_hba.conf.j2 /etc/postgresql/pg_hba.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/pg_ident.conf.j2 /etc/postgresql/pg_ident.conf
COPY --from=pgbase /src/supabase/ansible/files/postgresql_config/postgresql-stdout-log.conf /etc/postgresql/logging.conf
COPY --from=pgbase /src/supabase/migrations/db/ /docker-entrypoint-initdb.d/

RUN \
    mkdir -p /docker-entrypoint-initdb.d; \
    chmod 777 /docker-entrypoint-initdb.d; \
    /usr/share/postgresql/14/extension/pgsodium_getkey; \
    echo $PATH

VOLUME /var/lib/postgresql/data
STOPSIGNAL SIGINT
EXPOSE 5432
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres", "-c", "config_file=/etc/postgresql/postgresql.conf"]
