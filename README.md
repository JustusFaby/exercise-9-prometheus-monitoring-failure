# Exercise 9 - Prometheus Monitoring Failure

## Objective

Investigate and resolve a Prometheus monitoring issue where application metrics were not visible in Grafana.

## Scenario

### Grafana

No Data

### Prometheus

payment-service target unavailable

### ServiceMonitor

```yaml
port: metrics
```

### Service

```yaml
name: prometheus
```

## Investigation

### Verify Monitoring Stack

```bash
kubectl get pods -n monitoring
```

### Check ServiceMonitor

```bash
kubectl describe servicemonitor payment-service -n monitoring
```

### Check Service

```bash
kubectl get svc payment-service -o yaml
```

## Root Cause

The ServiceMonitor referenced:

```yaml
port: metrics
```

while the Kubernetes Service exposed:

```yaml
name: prometheus
```

Because the port names did not match, Prometheus could not discover and scrape the endpoint.

## Fix

Updated ServiceMonitor:

```yaml
port: prometheus
```

Added label:

```yaml
release: monitoring
```

## Demo Video

[Watch the demo video](https://drive.google.com/file/d/1zp81Y_4TeocHgm64U6ceP8Zqb5FctOts/view?usp=sharing)

## Verification

Prometheus Targets:

```
payment-service   UP
```

Grafana successfully received metrics from Prometheus.

## Technologies Used

* Kubernetes
* Minikube
* Prometheus
* Grafana
* ServiceMonitor
* Prometheus Operator

## Outcome

Successfully identified and resolved a metrics scraping issue by correcting ServiceMonitor configuration and validating target health through Prometheus.
