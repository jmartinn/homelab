# Monitoring Stack

## Overview

The monitoring stack consists of:
- Prometheus for metrics collection and storage
- Grafana for visualization
- Various exporters for system and service metrics

## Components

### Prometheus Stack
- **kube-prometheus-stack**: Comprehensive monitoring solution
  - Prometheus Operator
  - Node Exporter
  - kube-state-metrics
  - Default dashboards

### Grafana
- Multiple data sources
- Pre-configured dashboards
- Custom dashboards for specific services

### Configuration Structure

```
monitoring/
├── configs/
│   └── base/
│       ├── dashboards/             # Grafana dashboards
│       ├── grafana/                # Grafana configuration
│       └── prometheus/             # Prometheus configuration
└── controllers/
    └── base/
        ├── grafana/                # Grafana deployment
        └── kube-prometheus-stack/  # Prometheus deployment
```

## Dashboards

Currently configured dashboards:
- Cloudflare Tunnel metrics
- Node metrics
- Kubernetes cluster metrics
- System overview

## Metrics Collection

### System Metrics
- CPU usage
- Memory utilization
- Disk I/O
- Network statistics

### Kubernetes Metrics
- Pod status
- Node health
- Resource utilization
- Container metrics

### Application Metrics
- Service health
- Response times
- Error rates
- Custom metrics

## Access

- Grafana UI: https://grafana.yourdomain.net
- Default credentials in Grafana Helm values
- Secured through Cloudflare Tunnel

## Adding New Dashboards

1. Create dashboard JSON
2. Add to monitoring/configs/base/dashboards/
3. Update kustomization.yaml
4. Commit and push changes

## Common Operations

### Check Prometheus Status
```bash
kubectl get pods -n monitoring
```

### View Prometheus Targets
```bash
kubectl port-forward svc/prometheus-operated 9090:9090 -n monitoring
```

### Access Grafana Logs
```bash
kubectl logs -l app.kubernetes.io/name=grafana -n monitoring
```

## Troubleshooting

Common issues and solutions:
1. Missing metrics
   - Check service monitors
   - Verify label selectors
2. Dashboard not loading
   - Check Grafana logs
   - Verify data source configuration
3. Alert manager issues
   - Check alert manager configuration
   - Verify routing rules
