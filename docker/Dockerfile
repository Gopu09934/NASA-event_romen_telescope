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
        python3-pip && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy everything needed by the streaming script
COPY . .

RUN chmod +x start.sh

CMD ["./start.sh"]
