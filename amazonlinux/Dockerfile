ARG VERSION=2023

FROM amazonlinux:${VERSION} AS builder

RUN yum -y install \
      hostname \
      iproute \
      iputils \
      net-tools \
      openssh-server \
      procps-ng \
      sudo \
      systemd \
      wget \
    && yum clean all

FROM scratch
LABEL org.opencontainers.image.authors="MADIA <hello@malang.dev>"
LABEL org.opencontainers.image.source=https://github.com/malang-dev/microstack-image

ENV DEBIAN_FRONTEND=noninteractive
COPY --from=builder / /
