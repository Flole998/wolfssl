# This Dockerfile is used in conjunction with the docker-OpenWrt.yml GitHub Action.
FROM alpine:latest AS builder

RUN apk add argp-standalone asciidoc bash bc binutils bzip2 cdrkit coreutils \
  diffutils elfutils-dev findutils flex musl-fts-dev g++ gawk gcc gettext git \
  grep intltool libxslt linux-headers make musl-libintl musl-obstack-dev \
  ncurses-dev openssl-dev patch perl python3-dev rsync tar \
  unzip util-linux wget zlib-dev autoconf automake libtool
COPY . /workspace
RUN cd /workspace && ./autogen.sh && ./configure --enable-all && make

FROM openwrt/rootfs:x86-64-22.03.0

RUN mkdir -p /var/lock # Fix for parent container
RUN rm -f /usr/lib/libwolfssl* # make sure to eliminate existing wolfSSL library
COPY --from=builder /workspace/src/.libs/libwolfssl.so.35.3.0 /usr/lib/libwolfssl.so.5.4.0.ee39414e
COPY Docker/OpenWrt/runTests.sh /tmp/.
RUN /tmp/runTests.sh
