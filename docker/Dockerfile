FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV DISPLAY=:99

RUN apt-get update && \
    apt-get install -y \
        ffmpeg \
        curl \
        ca-certificates \
        xvfb \
        x11-utils \
        fonts-liberation \
        fonts-noto \
        python3 \
        python3-pip \
        nodejs \
        npm \
        chromium \
        libnss3 \
        libatk-bridge2.0-0 \
        libgtk-3-0 \
        libgbm1 \
        libasound2 \
        libxss1 \
        libxshmfence1 \
        libxcomposite1 \
        libxdamage1 \
        libxrandr2 \
        libxkbcommon0 && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY . .

RUN npm init -y && \
    npm install playwright

# Install Playwright browser WITHOUT --with-deps
RUN npx playwright install chromium

# Verify Node + Playwright
RUN node --version && \
    npm --version && \
    node -e "console.log(require.resolve('playwright'))" && \
    node -e "require('playwright'); console.log('PLAYWRIGHT OK')"

RUN chmod +x start.sh

CMD ["./start.sh"]
