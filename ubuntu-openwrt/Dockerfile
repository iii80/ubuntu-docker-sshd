FROM ubuntu:24.04
MAINTAINER "Hiroki Takeyama"

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Shanghai

# install dependencies
RUN apt update && apt install -y \
    tzdata \
    ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
    bzip2 ccache clang cmake cpio curl device-tree-compiler ecj fastjar flex gawk \
    gcc-multilib g++-multilib genisoimage gettext git gnutls-dev gperf haveged \
    help2man intltool lib32gcc-s1 libc6-dev-i386 libelf-dev libfuse-dev \
    libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev \
    libncurses-dev libncurses5-dev libncursesw5-dev libnsl-dev \
    libpython3-dev libreadline-dev libssl-dev libtool libyaml-dev \
    libz-dev lld llvm lrzsz mkisofs msmtp nano ninja-build \
    p7zip p7zip-full patch pkgconf python3 python3-docutils python3-pip \
    python3-ply python3-pyelftools python3-setuptools qemu-utils re2c \
    rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl \
    unzip vim wget xmlto xxd zlib1g-dev zstd rustc cargo golang \
    scdoc openssh-server sudo \
    && apt clean

# setup ssh
RUN mkdir /run/sshd \
    && sed -i 's/^#\(PermitRootLogin\) .*/\1 yes/' /etc/ssh/sshd_config \
    && sed -i 's/^\(UsePAM yes\)/# \1/' /etc/ssh/sshd_config

# create admin user
RUN useradd -m -s /bin/bash admin \
    && echo "admin:admin" | chpasswd \
    && usermod -aG sudo admin

# entrypoint
RUN echo '#!/bin/bash -eu' > /usr/local/bin/entry_point.sh && \
    echo 'ln -fs /usr/share/zoneinfo/${TZ} /etc/localtime' >> /usr/local/bin/entry_point.sh && \
    echo 'echo "root:${ROOT_PASSWORD}" | chpasswd' >> /usr/local/bin/entry_point.sh && \
    echo 'exec "$@"' >> /usr/local/bin/entry_point.sh && \
    chmod +x /usr/local/bin/entry_point.sh

ENV ROOT_PASSWORD=root

EXPOSE 22

ENTRYPOINT ["/usr/local/bin/entry_point.sh"]
CMD ["/usr/sbin/sshd", "-D", "-e"]
