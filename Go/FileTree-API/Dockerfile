# Build stage
FROM golang:1.22 AS builder

# Set the working directory
WORKDIR /app

# Copy the code from the GitHub Actions "Go/FileTree-API/source" directory
COPY source ./

# Download all dependencies. Dependencies will be cached if the go.mod and go.sum files are not changed
RUN go mod download

# Build the Go app
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o filetree cmd/server/main.go

# Production stage
FROM --platform=$TARGETPLATFORM alpine:latest

# Set the working directory
WORKDIR /root/

# Copy the Pre-built binary file from the previous stage
COPY --from=builder /app/filetree .

# Set the entry point
CMD ["./filetree"]
