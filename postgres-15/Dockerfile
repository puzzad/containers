# Generated from https://github.com/puzzad/containers/postgres-15/Dockerfile.gotpl
# BOM: {"apk:binutils":"2.40-r7","apk:brotli-libs":"1.0.9-r14","apk:busybox":"1.36.1-r1","apk:busybox-binsh":"1.36.1-r1","apk:ca-certificates":"20230506-r0","apk:curl":"8.2.1-r0","apk:g++":"12.2.1_git20220924-r10","apk:gcc":"12.2.1_git20220924-r10","apk:gmp":"6.2.1-r3","apk:isl25":"0.25-r2","apk:libacl":"2.3.1-r3","apk:libatomic":"12.2.1_git20220924-r10","apk:libc-dev":"0.7.2-r5","apk:libcrypto3":"3.1.1-r3","apk:libcurl":"8.2.1-r0","apk:libformw":"6.4_p20230506-r0","apk:libgcc":"12.2.1_git20220924-r10","apk:libgomp":"12.2.1_git20220924-r10","apk:libhistory":"8.2.1-r1","apk:libidn2":"2.3.4-r1","apk:libintl":"0.21.1-r7","apk:libmenuw":"6.4_p20230506-r0","apk:libncurses++":"6.4_p20230506-r0","apk:libncursesw":"6.4_p20230506-r0","apk:libpanelw":"6.4_p20230506-r0","apk:libssl3":"3.1.1-r3","apk:libstdc++":"12.2.1_git20220924-r10","apk:libstdc++-dev":"12.2.1_git20220924-r10","apk:libunistring":"1.1-r1","apk:libxxhash":"0.8.2-r0","apk:linux-headers":"6.3-r0","apk:lz4-libs":"1.9.4-r4","apk:make":"4.4.1-r1","apk:mpc1":"1.3.1-r1","apk:mpfr4":"4.2.0_p12-r0","apk:musl":"1.2.4-r0","apk:musl-dev":"1.2.4-r0","apk:musl-locales":"0.1.0-r1","apk:ncurses-dev":"6.4_p20230506-r0","apk:ncurses-terminfo-base":"6.4_p20230506-r0","apk:nghttp2-libs":"1.55.1-r0","apk:openssl-dev":"3.1.1-r3","apk:pkgconf":"1.9.5-r0","apk:popt":"1.19-r2","apk:readline":"8.2.1-r1","apk:readline-dev":"8.2.1-r1","apk:rsync":"3.2.7-r4","apk:scanelf":"1.3.7-r1","apk:zlib":"1.2.13-r1","apk:zlib-dev":"1.2.13-r1","apk:zstd-libs":"1.5.5-r4","image:ghcr.io/greboid/dockerfiles/alpine":"bb09baaefa0022cb709bfb42e8852359b208eb2a6baa0beff38eaa3a610141a7","image:ghcr.io/greboid/dockerfiles/base":"9e288621372d4df4cbfa7f4cb2a110fd0e7e00362bc156e902e808924388984b","postgres15":"15.3"}

FROM ghcr.io/greboid/dockerfiles/alpine@sha256:bb09baaefa0022cb709bfb42e8852359b208eb2a6baa0beff38eaa3a610141a7 as build

ARG ARCHIVE_URL="https://ftp.postgresql.org/pub/source/v15.3/postgresql-15.3.tar.bz2"
ARG ARCHIVE_SUM="ffc7d4891f00ffbf5c3f4eab7fbbced8460b8c0ee63c5a5167133b9e6599d932"

RUN set -eux; \
    apk add --no-cache \
        binutils=2.40-r7 \
        brotli-libs=1.0.9-r14 \
        busybox=1.36.1-r1 \
        busybox-binsh=1.36.1-r1 \
        ca-certificates=20230506-r0 \
        curl=8.2.1-r0 \
        g++=12.2.1_git20220924-r10 \
        gcc=12.2.1_git20220924-r10 \
        gmp=6.2.1-r3 \
        isl25=0.25-r2 \
        libacl=2.3.1-r3 \
        libatomic=12.2.1_git20220924-r10 \
        libc-dev=0.7.2-r5 \
        libcrypto3=3.1.1-r3 \
        libcurl=8.2.1-r0 \
        libformw=6.4_p20230506-r0 \
        libgcc=12.2.1_git20220924-r10 \
        libgomp=12.2.1_git20220924-r10 \
        libhistory=8.2.1-r1 \
        libidn2=2.3.4-r1 \
        libintl=0.21.1-r7 \
        libmenuw=6.4_p20230506-r0 \
        libncurses++=6.4_p20230506-r0 \
        libncursesw=6.4_p20230506-r0 \
        libpanelw=6.4_p20230506-r0 \
        libssl3=3.1.1-r3 \
        libstdc++=12.2.1_git20220924-r10 \
        libstdc++-dev=12.2.1_git20220924-r10 \
        libunistring=1.1-r1 \
        libxxhash=0.8.2-r0 \
        linux-headers=6.3-r0 \
        lz4-libs=1.9.4-r4 \
        make=4.4.1-r1 \
        mpc1=1.3.1-r1 \
        mpfr4=4.2.0_p12-r0 \
        musl=1.2.4-r0 \
        musl-dev=1.2.4-r0 \
        musl-locales=0.1.0-r1 \
        ncurses-dev=6.4_p20230506-r0 \
        ncurses-terminfo-base=6.4_p20230506-r0 \
        nghttp2-libs=1.55.1-r0 \
        openssl-dev=3.1.1-r3 \
        pkgconf=1.9.5-r0 \
        popt=1.19-r2 \
        readline=8.2.1-r1 \
        readline-dev=8.2.1-r1 \
        rsync=3.2.7-r4 \
        scanelf=1.3.7-r1 \
        zlib=1.2.13-r1 \
        zlib-dev=1.2.13-r1 \
        zstd-libs=1.5.5-r4 \
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

FROM ghcr.io/greboid/dockerfiles/base@sha256:9e288621372d4df4cbfa7f4cb2a110fd0e7e00362bc156e902e808924388984b

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
