FROM golang:1.21-alpine AS builder

WORKDIR /app
RUN apk add --no-cache git ca-certificates

COPY go.mod go.sum ./
RUN go mod download

COPY . ./
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o ivorioci-stream-service ./main.go

FROM alpine:3.18
RUN apk add --no-cache ca-certificates
WORKDIR /app

COPY --from=builder /app/ivorioci-stream-service ./ivorioci-stream-service
RUN mkdir -p /app/storage/videos /app/storage/thumbnails

EXPOSE 8080
CMD ["./ivorioci-stream-service"]
