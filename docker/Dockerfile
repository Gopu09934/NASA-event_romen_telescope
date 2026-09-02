FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# ffmpeg/curl/ca-certificates: original streaming toolchain.
# xvfb: virtual display so Chromium can render the NASA Eyes app
#   headfully (WebGL needs a real-ish display, not --headless mode)
#   without a physical screen.
# nodejs/npm: needed to install and run Playwright, which drives
#   Chromium to load/render the live tracker page.
#
# IMPORTANT: do NOT `rm -rf /var/lib/apt/lists/*` in this layer.
# `npx playwright install --with-deps` below needs to run its own
# `apt-get install` internally to pull Chromium's OS-level dependencies
# - if the apt package index is deleted here, that step fails.
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

# Split into separate RUN steps (instead of one chained command) so
# that if this breaks again, the Docker build log points at the exact
# failing step instead of one opaque combined command.
RUN npm init -y

# Pin a recent Playwright version explicitly. Ubuntu 24.04 ("noble")
# renamed several system libraries Chromium depends on (e.g.
# libasound2 -> libasound2t64). Older Playwright releases' dependency
# installer predates that rename and fails trying to apt-get install
# packages under the old names. A current version knows about noble's
# package names. If you were relying on whatever "latest" resolved to
# before, explicitly pinning also makes this build reproducible.
RUN npm install playwright@1.48.0

# Fresh apt index right before --with-deps needs it.
RUN apt-get update

# Install Chromium + its OS-level dependencies. --with-deps requires
# root and a working apt index (both true at this point in the image).
RUN npx playwright install --with-deps chromium

# Apt package index no longer needed after Playwright's install - clean
# it up in its own final layer.
RUN rm -rf /var/lib/apt/lists/*

RUN chmod +x start.sh

CMD ["./start.sh"]
