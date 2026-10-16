# Base images come from the workflow plane's registry mirror, which carries
# golang:1.25-alpine and alpine:latest only. Dependencies are vendored because
# the build has no egress to proxy.golang.org or the Alpine package CDN.
FROM golang:1.25-alpine AS builder
WORKDIR /app
COPY . .
ARG TARGETARCH
RUN CGO_ENABLED=0 GOOS=linux GOARCH=$TARGETARCH GOFLAGS=-mod=vendor GOTOOLCHAIN=local go build -o api-service .

# alpine ships ca-certificates-bundle, so no apk add is needed.
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/api-service .
EXPOSE 8080
CMD ["./api-service"]
