# ---------- Build stage ----------
FROM golang:1.22-alpine AS builder

WORKDIR /app

# Install git (needed for go modules sometimes)
RUN apk add --no-cache git

# Cache dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source
COPY . .

# Build static binary
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -o driftops ./cmd/driftops

# ---------- Runtime stage ----------
FROM gcr.io/distroless/base-debian12

WORKDIR /

# Copy binary only
COPY --from=builder /app/driftops /driftops

# Run as non-root
USER nonroot:nonroot

ENTRYPOINT ["/driftops"]
