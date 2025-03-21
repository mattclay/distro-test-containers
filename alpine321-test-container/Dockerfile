FROM public.ecr.aws/docker/library/alpine:3.21.2

# Keep this as basic as possible, so we properly test against BusyBox
# Things like tar/zip are needed by unarchive
# diffutils is installed for runme.sh tests that call diff
# coreutils was explicitly omitted
# sshpass was explicitly omitted as BusyBox timeout causes issues with our connection_ssh tests
RUN apk add --no-cache openrc \
        acl \
        bzip2 \
        curl \
        diffutils \
        file \
        gawk \
        gcc \
        git \
        libffi-dev \
        make \
        openssh-client \
        openssh-server \
        openssh-sftp-server \
        openssl-dev \
        python3 \
        python3-dev \
        py3-pip \
        py3-jinja2 \
        py3-cryptography \
        py3-lxml \
        py3-mock \
        py3-packaging \
        py3-resolvelib \
        py3-setuptools \
        py3-virtualenv \
        py3-yaml \
        py3-wheel \
        rsync \
        sudo \
        tar \
        tzdata \
        unzip \
        zip \
        musl-dev \
        yaml-dev \
        ca-certificates \
        iproute2-minimal \
        py3-passlib \
        pass \
        gnupg

RUN ssh-keygen -A
RUN rc-update add sshd
# inittab has some tty entries that cause issues
RUN sed -i '/getty/d' /etc/inittab
# The root account is locked by default, this effectively unlocks it
RUN echo root:ansible | chpasswd

COPY requirements.txt /usr/share/container-setup/requirements.txt
RUN pip install --break-system-packages --disable-pip-version-check --no-cache-dir -r /usr/share/container-setup/requirements.txt

ENV container=docker
CMD ["/sbin/init"]
