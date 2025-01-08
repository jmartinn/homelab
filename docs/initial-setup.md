# Installation Guide

This guide walks through setting up a homelab from scratch, including Kubernetes installation, GitOps configuration, and essential services.

## Prerequisites

- Ubuntu Server installed on your machine
- Domain name (for accessing services)
- Cloudflare account (for DNS and tunnels)
- GitHub account (for GitOps)

## Step 1: Base System Setup

1. Install Ubuntu Server
2. Install MicroK8s:
```bash
sudo snap install microk8s --classic
sudo usermod -a -G microk8s $USER
sudo chown -R $USER ~/.kube
newgrp microk8s
```

3. Enable required MicroK8s addons:
```bash
microk8s enable dns storage ingress
```

## Step 2: Local Development Setup

1. Install kubectl on your local machine:
```bash
brew install kubectl  # For macOS
```

2. Get kubeconfig from MicroK8s:
```bash
mkdir -p ~/.kube
ssh USERNAME@SERVER_IP 'microk8s config' > ~/.kube/config
```

3. Install Flux CLI:
```bash
brew install fluxcd/tap/flux
```

## Step 3: GitOps Setup

1. Create a new GitHub repository
2. Bootstrap Flux:
```bash
export GITHUB_USER=YOUR-USERNAME
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=homelab \
  --branch=main \
  --personal \
  --path=clusters/production
```

## Step 4: Network Setup

1. Create Cloudflare tunnel:
```bash
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared.deb
```

2. Authenticate and create tunnel:
```bash
cloudflared tunnel login
cloudflared tunnel create homelab-tunnel
```

3. Configure tunnel (in /etc/cloudflared/config.yml):
```yaml
tunnel: YOUR_TUNNEL_ID
credentials-file: /etc/cloudflared/YOUR_TUNNEL_ID.json
metrics: 0.0.0.0:2000
ingress:
  - hostname: grafana.yourdomain.net
    service: http://127.0.0.1:8001/api/v1/namespaces/monitoring/services/grafana:80/proxy
  - service: http_status:404
```

4. Configure tunnel as a service:
```bash
sudo cloudflared service install
```

## Step 5: Monitoring Stack

1. Configure namespace:
```bash
kubectl create namespace monitoring
```

2. Deploy monitoring stack through Flux using the configurations in:
- monitoring/controllers/base/kube-prometheus-stack
- monitoring/configs/base/prometheus
- monitoring/configs/base/grafana

## Step 6: Homepage Dashboard

1. Deploy Homepage through Flux using configurations in:
- apps/base/homepage

2. Configure Homepage settings in ConfigMap to display your services.

## Verification

1. Check all systems are running:
```bash
kubectl get pods -A
flux get all
```

2. Access services through your domain:
- https://grafana.yourdomain.net
- https://homepage.yourdomain.net

## Next Steps

- Set up additional services
- Configure backups
- Add custom Grafana dashboards
- Implement high availability
