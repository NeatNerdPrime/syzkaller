FROM golang:1.24-alpine AS dashboard-builder

WORKDIR /build

# Prepare the dependencies.
COPY go.mod ./
COPY go.sum ./
RUN go mod download
COPY pkg/gcs/ pkg/gcs/
COPY pkg/html/urlutil/ pkg/html/urlutil/

# Build the tool.
COPY syz-cluster/dashboard/ syz-cluster/dashboard/
COPY syz-cluster/pkg/ syz-cluster/pkg/
RUN go build -o /bin/web-dashboard /build/syz-cluster/dashboard

# Build the container.
FROM alpine:latest
WORKDIR /app

COPY --from=dashboard-builder /bin/web-dashboard /bin/web-dashboard

EXPOSE 8081

ENTRYPOINT ["/bin/web-dashboard"]
