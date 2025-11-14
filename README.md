# Prometheus

Scrapes metrics from OpenTelemetry Collector.

## Deploy

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus -f values.yaml -n observability --create-namespace
```

## Configure Collector URL

The collector URL is configured in `server.extraScrapeConfigs`. Override it:

```bash
helm upgrade prometheus prometheus-community/prometheus -f values.yaml \
  --set server.extraScrapeConfigs[0].static_configs[0].targets[0]="your-collector:8888" \
  -n observability
```

## Architecture

```
Services + Istio → OpenTelemetry Collector → Prometheus (scrape /metrics)
```

## Access

```bash
kubectl port-forward -n observability svc/prometheus-server 9090:80
```
