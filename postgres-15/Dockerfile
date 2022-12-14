# Generated from https://github.com/puzzad/containers/postgres-15/Dockerfile.gotpl
# BOM: {"apk:binutils":"2.39-r2","apk:brotli-libs":"1.0.9-r9","apk:busybox":"1.35.0-r29","apk:busybox-binsh":"1.35.0-r29","apk:ca-certificates":"20220614-r4","apk:curl":"7.87.0-r0","apk:g++":"12.2.1_git20220924-r4","apk:gcc":"12.2.1_git20220924-r4","apk:gmp":"6.2.1-r2","apk:isl24":"0.24-r1","apk:libacl":"2.3.1-r1","apk:libatomic":"12.2.1_git20220924-r4","apk:libc-dev":"0.7.2-r3","apk:libcrypto3":"3.0.7-r2","apk:libcurl":"7.87.0-r0","apk:libgcc":"12.2.1_git20220924-r4","apk:libgomp":"12.2.1_git20220924-r4","apk:libhistory":"8.2.0-r0","apk:libssl3":"3.0.7-r2","apk:libstdc++":"12.2.1_git20220924-r4","apk:libstdc++-dev":"12.2.1_git20220924-r4","apk:linux-headers":"5.19.5-r0","apk:lz4-libs":"1.9.4-r1","apk:make":"4.3-r1","apk:mpc1":"1.2.1-r1","apk:mpfr4":"4.1.0-r0","apk:musl":"1.2.3-r4","apk:musl-dev":"1.2.3-r4","apk:musl-locales":"0.1.0-r0","apk:ncurses-dev":"6.3_p20221119-r0","apk:ncurses-libs":"6.3_p20221119-r0","apk:ncurses-terminfo-base":"6.3_p20221119-r0","apk:nghttp2-libs":"1.51.0-r0","apk:openssl-dev":"3.0.7-r2","apk:pkgconf":"1.9.3-r0","apk:popt":"1.19-r0","apk:readline":"8.2.0-r0","apk:readline-dev":"8.2.0-r0","apk:rsync":"3.2.7-r0","apk:scanelf":"1.3.5-r1","apk:zlib":"1.2.13-r0","apk:zlib-dev":"1.2.13-r0","apk:zstd-libs":"1.5.2-r9","postgres15":"15.1"}

FROM ghcr.io/greboid/dockerfiles/alpine:latest as build

ARG ARCHIVE_URL="https://ftp.postgresql.org/pub/source/v15.1/postgresql-15.1.tar.bz2"
ARG ARCHIVE_SUM="64fdf23d734afad0dfe4077daca96ac51dcd697e68ae2d3d4ca6c45cb14e21ae"

RUN set -eux; \
    apk add --no-cache \
        binutils=2.39-r2 \
        brotli-libs=1.0.9-r9 \
        busybox=1.35.0-r29 \
        busybox-binsh=1.35.0-r29 \
        ca-certificates=20220614-r4 \
        curl=7.87.0-r0 \
        g++=12.2.1_git20220924-r4 \
        gcc=12.2.1_git20220924-r4 \
        gmp=6.2.1-r2 \
        isl24=0.24-r1 \
        libacl=2.3.1-r1 \
        libatomic=12.2.1_git20220924-r4 \
        libc-dev=0.7.2-r3 \
        libcrypto3=3.0.7-r2 \
        libcurl=7.87.0-r0 \
        libgcc=12.2.1_git20220924-r4 \
        libgomp=12.2.1_git20220924-r4 \
        libhistory=8.2.0-r0 \
        libssl3=3.0.7-r2 \
        libstdc++=12.2.1_git20220924-r4 \
        libstdc++-dev=12.2.1_git20220924-r4 \
        linux-headers=5.19.5-r0 \
        lz4-libs=1.9.4-r1 \
        make=4.3-r1 \
        mpc1=1.2.1-r1 \
        mpfr4=4.1.0-r0 \
        musl=1.2.3-r4 \
        musl-dev=1.2.3-r4 \
        musl-locales=0.1.0-r0 \
        ncurses-dev=6.3_p20221119-r0 \
        ncurses-libs=6.3_p20221119-r0 \
        ncurses-terminfo-base=6.3_p20221119-r0 \
        nghttp2-libs=1.51.0-r0 \
        openssl-dev=3.0.7-r2 \
        pkgconf=1.9.3-r0 \
        popt=1.19-r0 \
        readline=8.2.0-r0 \
        readline-dev=8.2.0-r0 \
        rsync=3.2.7-r0 \
        scanelf=1.3.5-r1 \
        zlib=1.2.13-r0 \
        zlib-dev=1.2.13-r0 \
        zstd-libs=1.5.2-r9 \
        ; \
    curl -sfLo postgresql.tar.bz2 $ARCHIVE_URL; \
    echo "$ARCHIVE_SUM *postgresql.tar.bz2" | sha256sum -wc -; \
    mkdir -p /usr/local/postgres; \
    tar -C /usr/local/postgres --strip-components 1 -xf postgresql.tar.bz2; \
    cd /usr/local/postgres; \
    ./configure \
        --with-system-tzdata=/usr/share/zoneinfo \
        --with-includes=/usr/local/include \
        --with-libraries=/usr/local/lib \
        --with-ssl=openssl; \
    make -j$(nproc) world; \
    make install-world; \
    make -C contrib install; \
    mkdir -p /src/data; \
    mkdir -p /src/run; \
    chmod 777 /src/run; \
    mkdir -p /etc/postgresql/;

RUN set -eux; \
    DEPS=$(scanelf --needed --nobanner --format '%n#p' --recursive -l /usr/local /usr/local/bin/psql \
    | tr ',' '\n' \
    | sort -u \
    | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }');\
    mkdir /rootfs; \
    for dep in $DEPS; do \
      apk add ${dep}; \
      apk info -qL ${dep} | rsync -aq --files-from=- / /rootfs/; \
    done;

FROM ghcr.io/greboid/dockerfiles/base:latest 

ENV LANG en_US.utf8
ENV PGDATA=/var/lib/postgres/data
ENV PGHOST=127.0.0.1
ENV PGUSER=postgres

COPY --from=build /bin/sh /bin/sh
COPY --from=build /bin/more /bin/more
COPY --from=build /usr/bin/locale /usr/bin/locale
COPY --from=build /rootfs/ /
COPY --from=build /usr/local/pgsql /usr/local/
COPY --from=build --chown=65532:65532 --chmod=750 /src/data /var/lib/postgres
COPY --from=build --chown=65532:65532 --chmod=777 /src/run /var/run/psql
COPY postgresql.conf /etc/postgresql.conf
COPY entry.sh /usr/local/bin/

STOPSIGNAL SIGINT

ENTRYPOINT ["/usr/local/bin/entry.sh"]
