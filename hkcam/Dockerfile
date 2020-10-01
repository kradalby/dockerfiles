FROM golang:latest AS build

RUN go get github.com/brutella/hkcam/cmd/hkcam
RUN go build -o /hkcam github.com/brutella/hkcam/cmd/hkcam


FROM ubuntu:latest

ENV HKCAM_HOMEKIT_PIN "00102003"
ENV HKCAM_MIN_BITRATE "500"
ENV HKCAM_ROTATION "0"
ENV HKCAM_MULTI_STREAM "false"

RUN apt-get update \
        && apt-get upgrade -y \
        && apt-get install -y v4l-utils ffmpeg libncurses5-dev bc \
        && rm -rf /var/lib/apt/lists/*

COPY --from=build /hkcam /usr/local/bin/hkcam

ENTRYPOINT [ "/usr/local/bin/hkcam" ]
