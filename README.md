
# Prometheus and Grafana Monitoring Setup

This repository provides a setup for monitoring system and node metrics using Prometheus and Grafana. The repository includes configurations for Prometheus, Grafana dashboards, and alerting rules to monitor the status of instances and system metrics effectively.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Prometheus Configuration](#prometheus-configuration)
- [Grafana Setup](#grafana-setup)
- [Alerts](#alerts)
- [License](#license)

## Overview

Prometheus is an open-source systems monitoring and alerting toolkit. Grafana is an open-source platform for monitoring and observability, offering beautiful visualizations of data.

This project includes:
- Prometheus setup to scrape metrics.
- Grafana dashboards to visualize these metrics.
- Alert rules to notify when any instance is down for more than 5 minutes.

## Features

- **Node Exporter Metrics**: Collects hardware and operating system metrics from *nix nodes.
- **Grafana Dashboards**: Provides rich visualizations for CPU, memory, disk, and network usage.
- **Alerting**: Sends alerts when instances are down for more than 5 minutes.
- **Modular Design**: Easily configurable and extendable setup for various environments.

## Project Structure

```bash
.
├── alert.rules.yml                # Prometheus alerting rules
├── LICENSE                        # License file
├── prometheus.yml                 # Prometheus configuration file
└── README.md                      # Project documentation (this file)

```

## Prerequisites

- **Prometheus**: Version 2.55.0 or later
- **Grafana**: Version 9.x or later
- **Node Exporter**: For collecting node metrics
- **Docker** (Optional): For containerized deployments

## Installation

### 1. Download Prometheus and Grafana

- [Prometheus Download](https://prometheus.io/download/)
- [Grafana Download](https://grafana.com/get/)

### 2. Configure Prometheus

1. Download Prometheus and extract it.
2. Copy `prometheus.yml` into your Prometheus directory.
3. Add node exporters for metrics scraping.

```bash
./prometheus --config.file=prometheus.yml
```

### 3. Start Grafana

Start Grafana to visualize Prometheus metrics.

```bash
grafana-server
```

### 4. Import Dashboards

1. Open Grafana and log in.
2. Go to **Dashboards** > **Manage** > **Import**.
3. Import your pre-configured dashboards (available in JSON format).

## Prometheus Configuration

Prometheus is configured to scrape metrics from the node exporter.

Sample configuration in `prometheus.yml`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
```

Ensure that the node exporter is running on the target systems.

## Alerts

Prometheus alert rules are defined in `alert.rules.yml`:

```yaml
groups:
  - name: node_alerts
    rules:
    - alert: InstanceDown
      expr: up == 0
      for: 5m
      labels:
        severity: critical
      annotations:
        summary: "Instance {{ $labels.instance }} down"
        description: "{{ $labels.instance }} has been down for more than 5 minutes."
```

The rule will trigger an alert if any instance is down for more than 5 minutes. Customize these rules as per your monitoring needs.

## License

This project is licensed under the [MIT License](./LICENSE).

