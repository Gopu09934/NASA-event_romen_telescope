FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV DISPLAY=:99

RUN apt-get update && \
    apt-get install -y \
        ffmpeg \
        curl \
        ca-certificates \
        xvfb \
        chromium \
        x11-utils \
        fonts-liberation \
        fonts-noto \
        python3 \
        python3-pip \
        wget \
        unzip \
        libnss3 \
        libatk-bridge2.0-0 \
        libgtk-3-0 \
        libgbm1 \
        libasound2t64 \
        libxss1 \
        libxshmfence1 \
        libxrandr2 \
        libxcomposite1 \
        libxdamage1 \
        libxfixes3 \
        libxkbcommon0 \
        libdrm2 && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY . .

RUN chmod +x start.sh

CMD ["./start.sh"]
