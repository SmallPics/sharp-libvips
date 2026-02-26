FROM arm64v8/amazonlinux:2023
LABEL maintainer="Lovell Fuller <npm@lovell.info>"

# Create Amazon Linux 2023 (glibc 2.34) container suitable for building Linux ARM64v8-A binaries

# Path settings
ENV \
  RUSTUP_HOME="/usr/local/rustup" \
  CARGO_HOME="/usr/local/cargo" \
  PATH="/usr/local/cargo/bin:$PATH"

# Build dependencies
RUN \
  dnf update -y && \
  dnf install -y --setopt=tsflags=nodocs \
    autoconf \
    automake \
    bzip2 \
    cmake \
    curl-devel \
    gcc \
    gcc-c++ \
    binutils \
    gettext \
    git \
    gperf \
    jq \
    make \
    openssl-devel \
    patch \
    pkgconfig \
    python3 \
    python3-pip \
    tar \
    xz \
    && \
  curl https://sh.rustup.rs -sSf | sh -s -- -y \
    --no-modify-path \
    --profile minimal \
    --default-host aarch64-unknown-linux-gnu \
    --default-toolchain nightly \
    && \
  cargo install cargo-c --locked && \
  pip3 install meson ninja tomli

# Compiler settings
ENV \
  PKG_CONFIG="pkg-config --static" \
  PLATFORM="linux-arm64v8" \
  FLAGS="-march=armv8-a" \
  RUSTFLAGS="-Zlocation-detail=none -Zfmt-debug=none" \
  MESON="--cross-file=/root/meson.ini"

COPY Toolchain.cmake /root/
COPY meson.ini /root/
