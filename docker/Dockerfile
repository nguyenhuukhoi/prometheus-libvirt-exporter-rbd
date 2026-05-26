FROM golang:1.22-bookworm AS build

WORKDIR /src

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -o /out/prometheus-libvirt-exporter-rbd .

FROM ubuntu:24.04

RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates ceph-common \
    && rm -rf /var/lib/apt/lists/*

COPY --from=build /out/prometheus-libvirt-exporter-rbd /usr/local/bin/prometheus-libvirt-exporter-rbd

ENTRYPOINT ["/usr/local/bin/prometheus-libvirt-exporter-rbd"]
