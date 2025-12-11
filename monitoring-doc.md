# Monitoring Documentation

## Required Metrics

1.  **Pod CPU Utilization**
    *   **Metric Name:** `container_cpu_usage_seconds_total`
    *   **Source:** cAdvisor (Standard Kubernetes Metric)
    *   **Description:** Measures CPU time consumed by the nginx/wordpress pods.

2.  **Total Request Count (Nginx)**
    *   **Metric Name:** `nginx_http_requests_total`
    *   **Source:** Nginx Prometheus Exporter (Sidecar)
    *   **Description:** Counts every incoming HTTP request to the Nginx proxy.

3.  **Total 5xx Requests**
    *   **Metric Name:** `nginx_http_requests_total{status=~"5.."}`
    *   **Source:** Nginx Prometheus Exporter
    *   **Description:** Filters request count for server errors (500, 502, 503, etc).
