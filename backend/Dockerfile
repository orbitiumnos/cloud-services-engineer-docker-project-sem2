# golang

FROM golang:1.25-alpine AS builder

WORKDIR /src

ENV CGO_ENABLED=0

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN go build -o /out/momo-store-api ./cmd/api
# -trimpath -ldflags="-s -w"

# linux

FROM alpine:3.22

#LABEL org.opencontainers.image.title="momo-store-backend" \
#      org.opencontainers.image.description="Go API backend for momo-store"

RUN apk upgrade --no-cache \
    && addgroup -S app \
    && adduser -S -G app -H -h /nonexistent app \
    && apk add --no-cache ca-certificates \
    && mkdir -p /app/data \
    && chown -R app:app /app

WORKDIR /app
COPY --from=builder --chown=app:app /out/momo-store-api ./momo-store-api

USER app

EXPOSE 8081

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD wget -q http://127.0.0.1:8081/health || exit 1
# -O /dev/null

ENTRYPOINT ["/app/momo-backend"]
