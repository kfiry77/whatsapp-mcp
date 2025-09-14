# Use official Golang image as build stage
FROM golang:alpine AS builder

WORKDIR /app

# Install git and gcc for go mod and cgo
RUN apk add --no-cache git gcc musl-dev

# Copy go.mod and go.sum
COPY go.mod go.sum ./
RUN go mod download

# Copy the source code
COPY . .

# Build the Go app with CGO enabled
RUN CGO_ENABLED=1 go build -o whatsapp-bridge main.go

# Final image
FROM alpine:latest

WORKDIR /app

# Install CA certificates (for HTTPS requests)
RUN apk add --no-cache ca-certificates

# Copy the built binary from builder
COPY --from=builder /app/whatsapp-bridge .

# Create store directory
RUN mkdir -p /app/store

# Expose REST API port
EXPOSE 8080

# Define volume for persistent storage
VOLUME ["/app/store"]

# Run the app
CMD ["./whatsapp-bridge"]