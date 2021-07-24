FROM golang:1 as build

ARG DRONE_VERSION=v2.0.4

ENV GOOS=linux
ENV GOARCH=amd64
ENV CGO_ENABLED=0
ENV GO111MODULE=on

WORKDIR /build
RUN git clone https://github.com/drone/drone \
    && cd drone \
    && git checkout $DRONE_VERSION \
    && go build -tags "nolimit" ./cmd/drone-server

FROM alpine:3.11 as alpine
RUN apk add -U --no-cache ca-certificates

FROM alpine:3.11
EXPOSE 80 443
VOLUME /data

RUN [ ! -e /etc/nsswitch.conf ] && echo 'hosts: files dns' > /etc/nsswitch.conf

ENV GODEBUG netdns=go
ENV XDG_CACHE_HOME /data
ENV DRONE_DATABASE_DRIVER sqlite3
ENV DRONE_DATABASE_DATASOURCE /data/database.sqlite
ENV DRONE_RUNNER_OS=linux
ENV DRONE_RUNNER_ARCH=amd64
ENV DRONE_SERVER_PORT=:80
ENV DRONE_SERVER_HOST=localhost

COPY --from=alpine /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

COPY --from=build /build/drone/drone-server /bin/drone-server
ENTRYPOINT ["/bin/drone-server"]
