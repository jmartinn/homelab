# Homelab

This repo contains all the configuration and documentation of my homelab.

The purpose of my homelab is to learn, to have fun and take advantage of what you build in the process. By self-hosting some applications, it makes me feel responsible for the entire process of deploying and maintaining an application from A to Z. It forces me to think about backup strategies, security, scalability and the ease of deployment and maintenance.

## Tooling

- k3s

## Goals

- Run Prometheus and Grafana stack as well as some databases
- Have Grafana dashboard available with a URL
    - ingress
    - tls
    - DNS
- Everything should be deployed using GitOps
    - Try out Flux
