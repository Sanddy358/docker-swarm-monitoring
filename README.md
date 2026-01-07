🚀 Docker Swarm Production Monitoring & Logging Setup

This project implements a production-grade monitoring and logging architecture using Docker Swarm with:

✅ Prometheus (metrics storage)

✅ Grafana (visualization)

✅ Loki (log storage)

✅ Promtail (log shipper)

✅ Grafana Alloy (unified agent / collector)

✅ cAdvisor (container metrics)

✅ JMX Exporter (Java application metrics)

🏗️ Architecture (IMPORTANT)

This setup uses TWO SERVER MODEL:

┌──────────────────────────────┐        ┌──────────────────────────────┐
│      Monitoring Server       │        │       Application Server      │
│                              │        │                              │
│  Grafana                     │ <────> │  Java Application            │
│  Prometheus                  │ <────> │  JMX Exporter                │
│  Loki                        │ <────> │  Promtail                    │
│                              │ <────> │  Grafana Alloy               │
│                              │ <────> │  cAdvisor                    │
└──────────────────────────────┘        └──────────────────────────────┘

🖥️ Server Responsibility
🖥️ 1️⃣ Monitoring Server

Install:

Grafana

Prometheus

Loki

🖥️ 2️⃣ Application Server

Install:

Java Application

JMX Exporter

Promtail

Grafana Alloy

cAdvisor

📁 Logs Directory

All application logs are stored in:

/backend/logs/


Promtail reads logs from:

__path__: /backend/logs/**/*.log

📁 Project Structure
monitoring/
├── docker-stack.yml
├── prometheus/
│   └── prometheus.yml
├── grafana/
│   ├── dashboards/
│   │   └── custom-dashboard.json
│   └── datasources/
├── loki/
│   └── loki-config.yml
├── promtail/
│   └── promtail-config.yml
├── alloy/
│   └── config.alloy
└── README.md

⚠️ VERY IMPORTANT FOR CLOUD DEPLOYMENT
🔁 Replace localhost With Private IP

In Prometheus config:

❌ WRONG:

targets: ['localhost:9100']


✅ CORRECT:

targets: ['172.31.10.170:9100']


📌 Because in cloud & Docker, localhost means inside container, not host machine.

🐳 Docker Swarm Deployment

Initialize swarm:

docker swarm init


Deploy stack:

docker stack deploy -c docker-stack.yml monitoring


Check services:

docker service ls

🌐 Access Dashboards
Service	URL
Grafana	http://<MONITORING-SERVER-IP>:3000
Prometheus	http://<MONITORING-SERVER-IP>:9090
Loki	http://<MONITORING-SERVER-IP>:3100

Grafana default:

admin / admin

📊 Grafana Dashboards
🔹 Option 1: Use Public Dashboard

Grafana Dashboard ID:

193

🔹 Option 2: Use Custom Dashboard (Recommended)

This repo contains:

grafana/dashboards/*.json


Before importing:

⚠️ You MUST update datasource UID

In JSON file:

Replace:

"uid": "PROMETHEUS_UID"


With:

"uid": "YOUR_DATASOURCE_UID"


Otherwise dashboard will show:

❌ Datasource not found

🔍 How To Find Datasource UID

Grafana → Settings → Data Sources → Prometheus → Copy UID

📜 Promtail Positions File
/tmp/positions.yaml


📌 Purpose:

Stores last read log line offset

Prevents duplicate logs after restart

DO NOT DELETE IN PRODUCTION

☕ JMX Exporter Setup (On Application Server)
1️⃣ Download JMX Jar

Download:

jmx_prometheus_javaagent-0.20.0.jar

2️⃣ Create Config File

Example:

jmx-config.yml

3️⃣ Start Java App With:
-javaagent:/jmx/jmx_prometheus_javaagent-0.20.0.jar=9404:/jmx/jmx-config.yml

4️⃣ In Prometheus Config:
- targets: ['<APP-SERVER-PRIVATE-IP>:9404']

🧠 Grafana Alloy

Grafana Alloy is used as:

Metrics collector

Logs collector

Forwarder to Prometheus & Loki

OpenTelemetry compatible agent

📦 Volume Mapping (IMPORTANT)

You MUST mount:

Application logs → /backend/logs/

Promtail → same path

cAdvisor → Docker socket

Alloy → host metrics paths

Otherwise metrics/logs will not appear.

🛠️ Verification Steps
✅ Check Prometheus:
Status → Targets → All must be UP

✅ Check Loki:

Grafana → Explore → Loki:

{job="promtail"}

✅ Check cAdvisor:
container_cpu_usage_seconds_total

⚠️ Common Mistakes
Mistake	Result
Using localhost in cloud	❌ No data
Wrong datasource UID	❌ Broken dashboard
No volume mapping	❌ No logs
Deleting positions file	❌ Duplicate logs
🏆 Why This Is Production-Grade

✅ Separate monitoring server

✅ Separate app server

✅ Centralized logs + metrics

✅ Scalable

✅ Cloud-ready

✅ Interview & resume ready DevOps project

👨💻 Author

Sanghpal Tayde
B.Tech AI & DS | DevOps | Cloud | MLOps | GenAI

⭐ If You Like This Project

Give it a ⭐ on GitHub 😄
