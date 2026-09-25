# syntax=docker/dockerfile:1

FROM debian:bookworm-slim

ARG TARGETARCH=amd64
ARG TARGETVARIANT=

WORKDIR /usr/src

ENV SPEECH_TO_PHRASE_VERSION=1.4.3

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        python3 \
        python3-pip \
        python3-dev \
        python3-venv \
        libopenblas0 \
        libencode-perl \
        curl \
        build-essential \
    && mkdir ./tools \
    && curl --location --output - \
        "https://huggingface.co/datasets/rhasspy/rhasspy-speech/resolve/main/tools/rhasspy-speech_${TARGETARCH}${TARGETVARIANT}.tar.gz?download=true" | \
        tar -C ./tools -xzf - \
    && python3 -m venv .venv \
    && apt-get remove --yes build-essential \
    && apt-get autoclean \
    && apt-get purge \
    && rm -rf /var/lib/apt/lists/*


COPY ./speech_to_phrase/ speech_to_phrase/
COPY pyproject.toml speech_to_phrase/
RUN /usr/src/.venv/bin/pip3 install --no-cache-dir /usr/src/speech_to_phrase

WORKDIR /
COPY docker/run.sh ./

EXPOSE 10300

ENTRYPOINT ["bash", "/run.sh"]
