# Barista Cafe App - DevOps Implementation

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)

## Infrastructure Overview

This repository contains the containerized implementation of the Barista Cafe web application with a complete DevOps pipeline. The application is a static website served through Nginx and containerized with Docker for consistent deployment across environments.

## DevOps Pipeline Architecture

### Containerization
- **Base Image**: `nginx:alpine` - Lightweight Nginx image based on Alpine Linux
- **Image Size**: ~25MB (optimized for fast deployment)
- **Port Exposed**: 80 (HTTP)
- **Configuration**: Default Nginx configuration with custom content

### CI/CD Implementation

```yaml
# Sample CI/CD Pipeline Structure
stages:
  - build
  - test
  - security_scan
  - deploy_staging
  - deploy_production
```

## Docker Implementation

### Dockerfile Analysis
```dockerfile
FROM nginx:alpine          # Base image: Alpine-based Nginx (small footprint)
WORKDIR /usr/share/nginx/html  # Set working directory to Nginx content root
COPY . /usr/share/nginx/html   # Copy all application files to container
EXPOSE 80                  # Expose HTTP port
CMD ["nginx", "-g", "daemon off;"]  # Run Nginx in foreground
```

### Build & Deployment Commands

```bash
# Build the Docker image
docker build -t barista-cafe-app:latest .

# Run locally for testing
docker run -d -p 8080:80 --name barista-cafe barista-cafe-app:latest

# Tag for container registry
docker tag barista-cafe-app:latest registry.example.com/barista-cafe-app:latest

# Push to container registry
docker push registry.example.com/barista-cafe-app:latest
```

## Infrastructure as Code

### Kubernetes Deployment

```yaml
# Sample Kubernetes deployment manifest
apiVersion: apps/v1
kind: Deployment
metadata:
  name: barista-cafe
spec:
  replicas: 3
  selector:
    matchLabels:
      app: barista-cafe
  template:
    metadata:
      labels:
        app: barista-cafe
    spec:
      containers:
      - name: barista-cafe
        image: registry.example.com/barista-cafe-app:latest
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: "0.5"
            memory: "512Mi"
          requests:
            cpu: "0.2"
            memory: "256Mi"
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 30
```

## Monitoring & Observability

- **Health Checks**: Nginx status endpoint monitoring
- **Metrics Collection**: Prometheus Nginx exporter
- **Logging**: Configured for JSON format and forwarded to centralized logging
- **Dashboard**: Grafana dashboard for visualizing traffic patterns

## Performance Optimization

- Alpine-based image for minimal footprint
- Nginx configured with gzip compression
- Static content served with appropriate cache headers
- Container resource limits defined to prevent resource exhaustion

## Security Measures

- Regular security scanning with Trivy
- Non-root user for Nginx process
- Read-only filesystem where possible
- Network policies to restrict traffic

## Local Development

```bash
# Start development environment
docker-compose up -d

# View logs
docker-compose logs -f

# Stop environment
docker-compose down
```

## Frontend Tech Stack
- HTML5, CSS3, Bootstrap 5, jQuery

## License
Template design by [Tooplate](https://www.tooplate.com/view/2137-barista-cafe)