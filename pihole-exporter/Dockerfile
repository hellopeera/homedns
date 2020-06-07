FROM golang:1.13-alpine as builder

WORKDIR /root/pihole-exporter

ARG PIHOLE_EXPORTER_VERSION=0.0.9
ARG PIHOLE_EXPORTER_SOURCE=https://github.com/eko/pihole-exporter/archive/

RUN \
  set -x && \
  apk add --no-cache curl && \
  curl -L ${PIHOLE_EXPORTER_SOURCE}${PIHOLE_EXPORTER_VERSION}.tar.gz -o /tmp/pihole-exporter.tar.gz && \
  tar xzf /tmp/pihole-exporter.tar.gz --strip 1 && \
  GO111MODULE=on go mod vendor && \
  GOOS=linux GOARCH=arm GOARM=7 go build -o pihole_exporter .


FROM alpine:3

COPY --from=builder /root/pihole-exporter/pihole_exporter /usr/local/bin/pihole_exporter

RUN \
  set -x && \
  apk update && \
  apk upgrade && \
  apk add tzdata curl

USER nobody

ENTRYPOINT ["/usr/local/bin/pihole_exporter"]
