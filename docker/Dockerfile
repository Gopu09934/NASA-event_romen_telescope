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

# Copy everything needed by the streaming script
COPY . .

# Install Playwright
RUN npm install playwright

# Install Playwright's Chromium dependencies/browser
RUN npx playwright install --with-deps chromium

RUN chmod +x start.sh

# Verify everything before starting the stream
RUN node --version && \
    npm --version && \
    node -e "require('playwright'); console.log('Playwright OK')" && \
    npx playwright --version

CMD ["./start.sh"]
