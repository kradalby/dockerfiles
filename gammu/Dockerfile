FROM ubuntu:22.04

RUN apt update; apt install gammu -y

WORKDIR /root

# COPY .gammurc /root/.gammurc

ENTRYPOINT ["/usr/bin/gammu", "-d", "text"]
