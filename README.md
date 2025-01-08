# Homelab

My personal homelab running on Kubernetes (microk8s) with GitOps practices using Flux CD.

## Components

- **Infrastructure**
  - Kubernetes: MicroK8s
  - GitOps: Flux CD
  - Ingress: Cloudflare Tunnel
  - SSL/TLS: Provided through Cloudflare
  - Secrets Management: Sealed Secrets

- **Monitoring**
  - Prometheus: Metrics collection and storage
  - Grafana: Metrics visualization and dashboards

- **User Interface**
  - Homepage: Central dashboard for services
  - Custom domain setup with SSL/TLS

## Getting Started

See the [installation guide](docs/installation.md) for step-by-step setup instructions.

## Documentation

- [Installation Guide](docs/installation.md)
- [Architecture Overview](docs/architecture.md)
- [Monitoring Stack](docs/monitoring.md)
- [Networking & Security](docs/networking.md)

## Goals

- [x] Run Prometheus and Grafana stack
- [x] External access with proper DNS and TLS
- [x] GitOps deployment with Flux
- [ ] Database management and backup strategies
- [ ] Additional self-hosted services
- [ ] Automated backup solutions
- [ ] High availability configurations
