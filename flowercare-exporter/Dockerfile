FROM --platform=linux/arm64 golang:1.19.5-alpine AS builder

ARG TARGETOS
ARG TARGETARCH

ENV GOOS=$TARGETOS
ENV GOARCH=$TARGETARCH

RUN apk add --no-cache make git bash bluez bluez-libs bluez-firmware

WORKDIR /build

COPY go.mod go.sum /build/
RUN go mod download
RUN go mod verify

COPY . /build/
RUN make build-binary

FROM --platform=linux/arm64 busybox
LABEL maintainer="Robert Jacob <xperimental@solidproject.de>"

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=builder /build/flowercare-exporter /usr/bin/flowercare-exporter

USER root
EXPOSE 9294

ENTRYPOINT ["/usr/bin/flowercare-exporter"]
