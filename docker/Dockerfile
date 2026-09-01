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
        nodejs \
        npm && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY . .

# Install Playwright Node package
RUN npm init -y && \
    npm install playwright

# Verify Node + npm + Playwright
RUN node --version && \
    npm --version && \
    node -e "console.log(require.resolve('playwright'))" && \
    node -e "require('playwright'); console.log('PLAYWRIGHT OK')"

RUN chmod +x start.sh

CMD ["./start.sh"]
