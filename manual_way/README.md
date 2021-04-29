# Install Prometheus in "manual" way

This method deploys Prometheus as a Statefulset with persistent storage metrics in a volume.

```yaml
  volumeClaimTemplates:
    - metadata:
        name: prometheus-metrics-db
      spec:
        accessModes:
        - ReadWriteOnce
        resources:
          requests:
            storage: 50Gi      
```
This folder assumes you are using Istio for Kubernetes ingress and creates a istio-ingress gateway for Grafana. Adapt to your ingress solution if you need.

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

## Deploy Prometheus and Alertmanager Statefulset

```bash
kubectl create -f prometheus-example.yaml
kubectl create -f alertmanager-deployment.yaml
```

## Grafana

### Create configmap to Grafana

```bash
cd manual_way/grafana
kubectl create -f .
```

### Install Grafana

```bash
helm install --name mygrafana stable/grafana --set sidecar.datasources.enabled=true --set sidecar.dashboards.enabled=true --set sidecar.datasources.label=grafana_datasource --set sidecar.dashboards.label=grafana_dashboard
```

### Create grafana gateway

```bash
cd ./manual_way/grafana
kubectl apply -f grafana-gateway.yaml
```

map your minikube ip to the address grafana.local on /etc/hosts

```bash
192.168.49.2 grafana.local
```

### Access Grafana

Get admin password:

```bash
kubectl get secret --namespace default mygrafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```


