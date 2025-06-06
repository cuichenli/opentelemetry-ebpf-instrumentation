# Build the testserver binary
# Docker command must be invoked from the projec root directory
FROM golang:1.24 AS builder

ARG TARGETARCH

ENV GOARCH=$TARGETARCH

WORKDIR /src

# Copy the go manifests and source
COPY vendor/ vendor/
COPY test/ test/
COPY go.mod go.mod
COPY go.sum go.sum

# Build
RUN go build -o testserver ./test/integration/components/testserver/testserver.go

# Create final image from minimal + built binary
FROM debian:bookworm-slim

WORKDIR /
RUN mkdir -p x509
COPY --from=builder /src/testserver .
COPY --from=builder /src/test/integration/components/testserver/x509/server_test_cert.pem x509
COPY --from=builder /src/test/integration/components/testserver/x509/server_test_key.pem x509
USER 0:0

CMD [ "/testserver" ]