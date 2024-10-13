ARG VERSION=11

FROM debian:${VERSION} AS builder
# udev is needed for booting a "real" VM, setting up the ttyS0 console properly
# kmod is needed for modprobing modules
# systemd is needed for running as PID 1 as /sbin/init
# Also, other utilities are installed
RUN apt-get update && apt-get install -y \
      curl \
      wget \
      dbus \
      kmod \
      iproute2 \
      iputils-ping \
      net-tools \
      openssh-server \
      rng-tools \
      sudo \
      systemd \
      udev \
      nano \
      htop && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create the following files, but unset them
RUN echo "" > /etc/machine-id && \
    echo "" > /var/lib/dbus/machine-id

# Set dns resolver
COPY resolv.conf /etc/resolv.conf

# This container image doesn't have locales installed. Disable forwarding the
# user locale env variables or we get warnings such as:
#  bash: warning: setlocale: LC_ALL: cannot change locale
RUN sed -i -e 's/^AcceptEnv LANG LC_\*$/#AcceptEnv LANG LC_*/' /etc/ssh/sshd_config

# Set the root password to root when logging in through the VM's ttyS0 console
RUN echo "root:root" | chpasswd

FROM scratch
LABEL org.opencontainers.image.authors="MADIA <hello@malang.dev>"
LABEL org.opencontainers.image.source=https://github.com/malang-dev/microstack-image

ENV DEBIAN_FRONTEND=noninteractive
COPY --from=builder / /
