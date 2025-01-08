```markdown
# Homelab Commands Reference

## Flux Commands
```bash
# View the status of your Kustomizations
flux get kustomizations

# View the status of all Flux resources
flux get all

# Force Flux to sync with Git repository
flux reconcile source git flux-system

# View Flux logs
flux logs
```

## Kubernetes Basic Operations
```bash
# Get resources across all namespaces
kubectl get all -A
kubectl get pods -A
kubectl get services -A

# Get resources in specific namespace
kubectl get pods -n monitoring
kubectl get services -n monitoring

# View detailed information
kubectl describe pod <pod-name> -n <namespace>
kubectl describe service <service-name> -n <namespace>

# View logs
kubectl logs <pod-name> -n <namespace>
kubectl logs -f <pod-name> -n <namespace>  # Follow logs

# Execute commands in pods
kubectl exec -it <pod-name> -n <namespace> -- /bin/sh
```

## Service Management
```bash
# Restart deployments
kubectl rollout restart deployment <deployment-name> -n <namespace>
kubectl rollout restart deployment homepage -n homepage

# Scale deployments
kubectl scale deployment <deployment-name> -n <namespace> --replicas=<number>

# Check rollout status
kubectl rollout status deployment <deployment-name> -n <namespace>
```

## Secret Management
```bash
# Create a secret
kubectl create secret generic <secret-name> \
  --namespace <namespace> \
  --from-literal=key=value

# View secrets
kubectl get secrets -n <namespace>
kubectl get secret <secret-name> -n <namespace> -o yaml

# Delete secrets
kubectl delete secret <secret-name> -n <namespace>
```

## Namespace Operations
```bash
# List namespaces
kubectl get namespaces

# Create namespace
kubectl create namespace <name>

# Delete namespace (and all resources in it)
kubectl delete namespace <name>
```

## ConfigMap Operations
```bash
# View ConfigMaps
kubectl get configmaps -n <namespace>
kubectl describe configmap <name> -n <namespace>
kubectl get configmap <name> -n <namespace> -o yaml
```

## Cloudflare Tunnel
```bash
# Restart cloudflared service
sudo systemctl restart cloudflared

# View cloudflared logs
sudo journalctl -u cloudflared -f

# View tunnel metrics
curl localhost:2000/metrics
```

## Monitoring
```bash
# Check Prometheus targets
kubectl port-forward -n monitoring svc/prometheus-operated 9090:9090
# Then visit localhost:9090 in browser

# Access Grafana locally
kubectl port-forward -n monitoring svc/grafana 3000:80
```

## Debug Commands
```bash
# Check DNS resolution
kubectl run tmp-shell --rm -i --tty --image nicolaka/netshoot -- bash

# View events in namespace
kubectl get events -n <namespace>

# Resource usage
kubectl top pods -n <namespace>
kubectl top nodes
```

## Common Troubleshooting Patterns
```bash
# Check if pod is running
kubectl get pods -n <namespace> | grep <pod-name>

# Get pod failure reasons
kubectl describe pod <pod-name> -n <namespace> | grep -A 10 "Events:"

# Container logs prior to crash
kubectl logs <pod-name> -n <namespace> --previous

# Resource constraints
kubectl describe nodes | grep -A 5 "Allocated resources"
```

## Useful Aliases
Add these to your `~/.bashrc` or `~/.zshrc`:
```bash
alias k='kubectl'
alias kgp='kubectl get pods'
alias kgpa='kubectl get pods -A'
alias kgs='kubectl get services'
alias kgn='kubectl get nodes'
alias kd='kubectl describe'
alias kl='kubectl logs'
```

Note: Replace placeholders like `<namespace>`, `<pod-name>`, etc. with actual values when using these commands.
