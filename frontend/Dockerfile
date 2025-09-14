FROM golang:1.17-alpine AS build

WORKDIR /app
COPY go.mod .
RUN go mod download
COPY . .
ENV CGO_ENABLED=0
RUN go build \
    -trimpath \
    -ldflags="-s -w" \
    -o /out/app ./cmd/api

FROM alpine:3.15
COPY --from=build /out/app /app
RUN adduser -D -s /sbin/nologin testuser && \
    chown testuser:testuser /app && chmod +x /app
USER testuser
EXPOSE 8081

HEALTHCHECK --interval=20s --timeout=3s --start-period=5s --retries=3 \
    CMD wget -qO- http://127.0.0.1:8081/store || exit 1

ENTRYPOINT ["/app"]
