# First stage: Install dependencies
FROM debian:latest AS builder

# Install dependencies
RUN apt-get update && apt-get install -y \
    sudo \
	curl \
    git \
    python3 \
    python3-dev \
    python3-setuptools \
    ca-certificates \
    which \
    sed \
    make \
    binutils \
    build-essential \
    diffutils \
    gcc \
    g++ \
    bash \
    patch \
    gzip \
    bzip2 \
    perl \
    tar \
    cpio \
    unzip \
    rsync \
    file \
    bc \
    findutils \
    wget \
    libncurses5 \
    libncurses5-dev \
    libgnutls28-dev \
    libssl-dev \
    gcc-arm-linux-gnueabihf \
    bison \
    flex \
    u-boot-tools \
    swig \
    genimage \
	cmake \
    && apt-get clean && rm -rf /var/lib/apt/lists/*


# Second layer
FROM builder AS buildroot

# Create non-root user 'builduser' with password 'builduser'
RUN useradd -m -s /bin/bash builduser && \
    echo "builduser:builduser" | chpasswd && \
    usermod -aG sudo builduser

# Creating working directory and setting permissions
RUN mkdir -p /opt/work && chown builduser:builduser /opt/work

# Switching to builduser
USER builduser
WORKDIR /opt/work

RUN git clone --branch 2025.02 https://github.com/buildroot/buildroot.git /opt/work/buildroot