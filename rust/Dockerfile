# Generated from https://github.com/greboid/dockerfiles/blob/master/rust/Dockerfile.gotpl
# BOM: {"github:rust-lang/rust":"1.65.0"}

FROM debian:stable-slim as deb

ARG TAG1.65.0

ENV RUSTUP_HOME=/opt/rust
ENV CARGO_HOME=/opt/rust
ENV PATH=/opt/rust/bin:$PATH

RUN set -eux; \
     apt update; \
     apt install -y --no-install-recommends --no-install-suggests \
        gpg \
        lsb-release \
        ca-certificates \
        libsodium23 \
        curl; \
    curl -Ss https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init -o rustup-init; \
    chmod +x rustup-init; \
    ./rustup-init -y --default-toolchain stable --profile minimal --no-modify-path;
    rm -rf /var/lib/apt/lists/*; \
