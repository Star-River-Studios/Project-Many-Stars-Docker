# 基于Debian 13（Trixie）作为基础镜像
FROM debian:trixie

# 镜像基本信息
ARG VERSION
LABEL org.opencontainers.image.title="Many Stars MC"
LABEL org.opencontainers.image.description="Project Many Stars Minecraft Universal Docker Image"
LABEL org.opencontainers.image.authors="ManyStars"
LABEL org.opencontainers.image.vendor="Star River Studios"
LABEL org.opencontainers.image.source="https://github.com/Star-River-Studios/Project-Many-Stars-Docker"
LABEL org.opencontainers.image.licenses="MIT"
LABEL org.opencontainers.image.version="${VERSION}"

# 设置环境变量
ENV TERM="xterm-256color"
ENV DEBIAN_FRONTEND="noninteractive"
ENV TZ="Asia/Shanghai"
ENV LANG="en_US.utf8"
ENV LC_ALL="en_US.utf8"

# 安装系统组件
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list.d/debian.sources \
    && apt update \
    && apt dist-upgrade -y --no-install-recommends \
    && apt install -y --no-install-recommends gnupg ca-certificates curl locales-all apt-utils \
    && sed -i 's|^URIs: http://|URIs: https://|' /etc/apt/sources.list.d/debian.sources \
    && apt clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone

# 安装程序组件
RUN curl -s https://repos.azul.com/azul-repo.key | gpg --dearmor -o /usr/share/keyrings/azul.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/azul.gpg] https://repos.azul.com/zulu/deb stable main" > /etc/apt/sources.list.d/zulu.list \
    && apt update \
    && apt install -y --no-install-recommends zulu21-jre python3 python3-pip \
    && apt clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && python3 -m pip install --no-cache-dir --break-system-packages mcdreforged

# 创建默认工作目录
RUN  mkdir -p /app/mc-server
WORKDIR /app/mc-server