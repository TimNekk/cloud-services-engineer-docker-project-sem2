ARG GO_VERSION=1.22.5
ARG ALPINE_VERSION=3.20

FROM golang:${GO_VERSION}-alpine${ALPINE_VERSION} AS deps
WORKDIR /src
RUN apk add --no-cache ca-certificates wget

COPY go.mod go.sum ./
RUN go mod download

FROM deps AS builder
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -trimpath -ldflags="-s -w" -o /out/momo-api ./cmd/api

FROM deps AS development
WORKDIR /src
COPY . .
EXPOSE 8081
CMD ["go", "run", "./cmd/api"]

FROM alpine:${ALPINE_VERSION} AS runtime
RUN addgroup -S app && adduser -S -G app app \
    && apk add --no-cache ca-certificates wget

WORKDIR /app
COPY --from=builder /out/momo-api /app/momo-api

USER app
EXPOSE 8081

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget -q -O /dev/null http://127.0.0.1:8081/health || exit 1

ENTRYPOINT ["/app/momo-api"]
