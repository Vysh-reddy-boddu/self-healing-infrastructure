# 🧠 Self-Healing Infrastructure using Prometheus, Alertmanager & Flask

This project demonstrates an automated **self-healing monitoring setup** using **Prometheus**, **Alertmanager**, and a **Flask webhook**.  
When an issue like a node or service failure is detected, an alert is triggered, and the system automatically executes a recovery script.

---

## ⚙️ Architecture Overview

Prometheus → Alertmanager → Flask Webhook → Recovery Script (e.g., restart_nginx.yml)


1. **Prometheus** continuously scrapes metrics from Node Exporter.  
2. If the target (e.g., Node Exporter) goes down, it triggers an alert defined in `alert_rules.yml`.  
3. **Alertmanager** receives the alert and sends a POST request to the Flask **webhook** (`webhook.py`).  
4. The webhook executes the **self-healing action**, like restarting a service automatically using Ansible or shell scripts.

---

## 📁 Repository Structure

self-healing-infrastructure/
│
├── prometheus.yml # Prometheus configuration
├── alert.rules.yml # Prometheus alert rules
├── alertmanager.yml # Alertmanager configuration
├── restart_nginx.yml # Sample self-healing Ansible playbook
├── webhook.py # Flask webhook server
└── README.md # Project documentation


---

## 🧩 Setup Instructions

### 1️⃣ Start Node Exporter
```bash
./node_exporter &

### 1️⃣ SRun Prometheus
```bash
./prometheus --config.file=prometheus.yml

### 1️⃣ SRun Alertmanager
```bash
./alertmanager --config.file=alertmanager.yml

### 1️⃣ SStart the Flask Webhook
```bash
python3 webhook.py

### 1️⃣ Test the Setup
Stop Node Exporter:
```bash
sudo pkill node_exporter

Prometheus detects it → Alertmanager sends webhook → Flask executes recovery.
