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
        npm && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY . .

RUN npm init -y
RUN npm install playwright
RUN npx playwright install --with-deps chromium

# HARD verification
RUN node -e "console.log('Node:', process.version)"
RUN node -e "console.log('Playwright:', require.resolve('playwright'))"
RUN node -e "require('playwright'); console.log('PLAYWRIGHT OK')"

RUN chmod +x start.sh

CMD ["./start.sh"]
