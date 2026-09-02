FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# ffmpeg/curl/ca-certificates: original streaming toolchain.
# xvfb: virtual display so Chromium can render the NASA Eyes app
#   headfully (WebGL needs a real-ish display, not --headless mode)
#   without a physical screen.
# nodejs/npm: needed to install and run Playwright, which drives
#   Chromium to load/render the live tracker page.
# NOTE: this significantly increases image size and build time vs the
# original (Playwright's Chromium + its system deps are large). If the
# live tracker is disabled (ENABLE_LIVE_TRACKER_VIDEO=false at runtime),
# these packages just go unused — the stream.sh fallback path still
# works, but the image cost is paid either way at build time.
RUN apt-get update && \
    apt-get install -y \
        ffmpeg \
        curl \
        ca-certificates \
        xvfb \
        nodejs \
        npm && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy the entire build context.
# This automatically includes:
# start.sh, overlay.png, font.ttf, galaxy_info.txt,
# facts.txt, and any per-video text files.
COPY . .

# Install Playwright and download Chromium + its OS-level dependencies
# (fonts, GTK, audio libs, etc. — this is what --with-deps handles).
# Kept as a separate layer from COPY . . above it so that changing your
# script/asset files doesn't force a full Chromium re-download on every
# rebuild.
RUN npm init -y && \
    npm install playwright && \
    npx playwright install --with-deps chromium

RUN chmod +x start.sh

CMD ["./start.sh"]
