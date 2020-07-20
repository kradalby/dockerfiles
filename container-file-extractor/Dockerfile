FROM ubuntu:latest

RUN apt-get update \
        && apt-get upgrade -y \
        && apt-get install -y curl jq gzip tar \
        && rm -rf /var/lib/apt/lists/*

COPY container-file-extractor /usr/local/bin/
RUN chmod +x /usr/local/bin/container-file-extractor
