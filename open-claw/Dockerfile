FROM node:24-bookworm

# Build-time: Compose passes ${OPENCLAW_NPM_VERSION} from .env via build.args or:
# docker build --build-arg OPENCLAW_NPM_VERSION=1.2.3 -t ...
ARG OPENCLAW_NPM_VERSION=latest

ENV DEBIAN_FRONTEND=noninteractive
ENV OPENCLAW_DISABLE_BONJOUR=1

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates curl git jq zsh bash sudo \
    python3 python3-pip build-essential \
    ffmpeg yt-dlp tesseract-ocr \
  && rm -rf /var/lib/apt/lists/*

RUN npm i -g "openclaw@${OPENCLAW_NPM_VERSION}"

RUN useradd -m -s /bin/zsh openclaw \
  && mkdir -p /home/openclaw/.openclaw \
  && chown -R openclaw:openclaw /home/openclaw

USER openclaw
WORKDIR /home/openclaw

EXPOSE 18789

CMD ["openclaw", "gateway", "--bind", "lan", "--port", "18789"]
