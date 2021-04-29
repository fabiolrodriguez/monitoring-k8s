# Operator way

Installation

```bash
git clone https://github.com/prometheus-operator/kube-prometheus
kubectl create -f kube-prometheus/manifests/setup
kubectl create -f kube-prometheus/manifests/
```

```bash
helm install --name coredns --namespace=coredns stable/coredns
helm install  coredns --namespace=coredns stable/coredns
```

```bash
kubectl create -f servicemonitor-coredns.yaml
```