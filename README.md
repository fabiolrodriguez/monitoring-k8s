# Monitoring K8S

This is a collection of scripts to monitor Kubernetes Clusters and Applications

Here are exposed 3 methods to deploy a observability environment with Prometheus + Grafana + Alertmanager

## Helm way

The simple one, describing how to install Prometheus via Helm and monitor a single Redis deployment. Here you learn to install Minikube and Istio too, case you want.

## Manual way

Deploys the complete stack via Statefulset, with persistent metrics storage and ingress for Grafana.

## Operator way

Deploys the complete stack via Prometheus Operator - WIP