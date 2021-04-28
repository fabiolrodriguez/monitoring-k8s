# Install Prometheus in "manual" way

## Pre requisites

Install prometheus node_exporter:

```bash
helm install prometheus-node-exporter prometheus-community/prometheus-node-exporter
```
Install kube-state-metrics:

```bash
it clone https://github.com/kubernetes/kube-state-metrics.git
kubectl apply -f examples/standard
```

## Create configmaps for Prometheus and Alert Manager

```bash
cd manual_way
kubectl create cm prometheus-example-cm --from-file prometheus.yml
kubectl create cm prometheus-rules-general --from-file generalrules.yaml
kubectl create cm alertmanager-config --from-file alertmanager.yml
```

## Deploy Prometheus and Alertmanager

```bash
kubectl create -f prometheus-example.yaml
kubectl create -f alertmanager-deployment.yaml
```
## Create configmap to Grafana

```bash
cd manual_way/grafana
kubectl create -f .
```

## Install Grafana

```bash
helm install --name mygrafana stable/grafana --set sidecar.datasources.enabled=true --set sidecar.dashboards.enabled=true --set sidecar.datasources.label=grafana_datasource --set sidecar.dashboards.label=grafana_dashboard
```

## Access Grafana

Get admin password:

```bash
kubectl get secret --namespace default mygrafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
