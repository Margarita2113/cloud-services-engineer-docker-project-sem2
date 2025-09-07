FROM golang:1.20-alpine AS builder
WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 go build -o /app/bin/app ./cmd/api

FROM alpine:3.17
RUN addgroup -S appgroup && adduser -S testuser -G appgroup
WORKDIR /app
COPY --from=builder /app/bin/app ./app
USER testuser
EXPOSE 8081

CMD ["./app"]
