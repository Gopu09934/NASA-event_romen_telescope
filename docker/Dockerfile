FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y \
        ffmpeg \
        curl \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy the entire build context.
# This automatically includes:
# start.sh, overlay.png, font.ttf, galaxy_info.txt,
# facts.txt, and any per-video text files.
COPY . .

RUN chmod +x start.sh

CMD ["./start.sh"]
