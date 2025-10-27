# Self-Healing Infrastructure

This project implements a self-healing infrastructure using Prometheus, Alertmanager, Blackbox Exporter, and a Flask-based webhook with Ansible to monitor an NGINX container and automatically restart it when it goes down. The system detects HTTP failures via the `NginxHttpDown` alert and triggers an Ansible playbook to restart the `self-healing-infrastructure-nginx-1` container.

## Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
- [Testing the Self-Healing Mechanism](#testing-the-self-healing-mechanism)
- [Troubleshooting](#troubleshooting)

## Overview
The self-healing infrastructure monitors an NGINX container using Prometheus and Blackbox Exporter. When the NGINX HTTP endpoint is down for more than 1 minute, Prometheus triggers the `NginxHttpDown` alert, which is sent to Alertmanager. Alertmanager forwards the alert to a Flask webhook, which runs an Ansible playbook to restart the NGINX container. The system is deployed using Docker Compose.

## Architecture
- **NGINX**: Serves HTTP content and is monitored for availability.
- **Prometheus**: Scrapes metrics from Blackbox Exporter and evaluates alerting rules.
- **Blackbox Exporter**: Probes the NGINX HTTP endpoint (`http://nginx:80`) to check availability.
- **Alertmanager**: Receives alerts from Prometheus and forwards them to the webhook.
- **Webhook**: A Flask application that receives alerts and runs an Ansible playbook.
- **Ansible**: Executes the `restart_nginx.yaml` playbook to restart the NGINX container via the Docker API.

## Prerequisites
- **Docker**: Version 20.10 or higher.
- **Docker Compose**: Version 2.0 or higher.
- **Python**: Version 3.11 (used in the webhook container).
- **Git**: For cloning the repository.
- **Linux Host**: Recommended for Docker socket access (`/var/run/docker.sock`).

## Project Structure
```
    Self-Healing Infrastructure
    ├── alertmanager/
    │   └── alertmanager.yaml
    ├── playbooks/
    │   └── restart\_nginx.yaml
    ├── prometheus/
    │   ├── prometheus.yaml
    │   └── alerts.yaml
    ├── webhook/
    │   ├── app.py
    │   ├── Dockerfile
    │   └── entrypoint.sh
    ├── docker-compose.yaml
    └── README.md
```

