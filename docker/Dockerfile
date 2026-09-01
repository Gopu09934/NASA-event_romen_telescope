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
        gnupg && \
    rm -rf /var/lib/apt/lists/*

# Node.js 22 for GitHub Actions / tracker browser
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get update && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY . .

RUN chmod +x start.sh

# Verify required software during Docker build
RUN node --version && \
    npm --version && \
    chromium --version && \
    Xvfb -version

CMD ["./start.sh"]
