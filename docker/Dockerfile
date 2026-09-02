FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# ffmpeg/curl/ca-certificates: original streaming toolchain.
# xvfb: virtual display so Chromium can render the NASA Eyes app
#   headfully (WebGL needs a real-ish display, not --headless mode)
#   without a physical screen.
# nodejs/npm: needed to install and run Playwright, which drives
#   Chromium to load/render the live tracker page.
#
# NOTE: this significantly increases image size and build time vs the
# original (Playwright's Chromium + its system deps are large). If the
# live tracker is disabled (ENABLE_LIVE_TRACKER_VIDEO=false at runtime),
# these packages just go unused - the stream.sh fallback path still
# works, but the image cost is paid either way at build time.
#
# IMPORTANT: do NOT `rm -rf /var/lib/apt/lists/*` in this layer.
# `npx playwright install --with-deps` below needs to run its own
# `apt-get install` internally to pull Chromium's OS-level dependencies
# (fonts, GTK, audio libs, etc.) - if the apt package index is deleted
# here, that step fails with exit code 1. The index is cleaned up in
# the final apt-get layer instead, after Playwright no longer needs it.
RUN apt-get update && \
    apt-get install -y \
        ffmpeg \
        curl \
        ca-certificates \
        xvfb \
        nodejs \
        npm

WORKDIR /app

# Copy the entire build context.
# This automatically includes:
# start.sh, overlay.png, font.ttf, galaxy_info.txt,
# facts.txt, and any per-video text files.
COPY . .

# Install Playwright and download Chromium + its OS-level dependencies.
# Re-run apt-get update first since some time may have passed since the
# base layer, guaranteeing a fresh index for --with-deps to use.
RUN npm init -y && \
    npm install playwright && \
    apt-get update && \
    npx playwright install --with-deps chromium

# Apt package index is no longer needed after this point - clean it up
# here (final layer that touches apt) instead of earlier, so Playwright's
# --with-deps step above still had a working index to install from.
RUN rm -rf /var/lib/apt/lists/*

RUN chmod +x start.sh

CMD ["./start.sh"]
