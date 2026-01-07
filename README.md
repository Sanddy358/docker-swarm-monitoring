# 🚀 Docker Swarm Production Monitoring & Logging Setup

This project implements a **production-grade monitoring and logging architecture** using Docker Swarm with:

- ✅ Prometheus (metrics storage)
- ✅ Grafana (visualization)
- ✅ Loki (log storage)
- ✅ Promtail (log shipper)
- ✅ Grafana Alloy (unified agent / collector)
- ✅ cAdvisor (container metrics)
- ✅ JMX Exporter (Java application metrics)

---

# 🏗️ Architecture (IMPORTANT)

This setup uses **TWO SERVER MODEL**:

┌──────────────────────────────┐ ┌──────────────────────────────┐
│ Monitoring Server │ │ Application Server │
│ │ │ │
│ Grafana │ <────> │ Java Application │
│ Prometheus │ <────> │ JMX Exporter │
│ Loki │ <────> │ Promtail │
│ │ <────> │ Grafana Alloy │
│ │ <────> │ cAdvisor │
└──────────────────────────────┘ └──────────────────────────────┘

---

# 🖥️ Server Responsibility

## 🖥️ 1️⃣ Monitoring Server

Install:

- Grafana  
- Prometheus  
- Loki  

## 🖥️ 2️⃣ Application Server

Install:

- Java Application  
- JMX Exporter  
- Promtail  
- Grafana Alloy  
- cAdvisor  

---

# 📁 Logs Directory

All application logs are stored in:

```bash
/backend/logs/

Promtail reads logs from:

__path__: /backend/logs/**/*.log

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

⚠️ VERY IMPORTANT FOR CLOUD DEPLOYMENT
🔁 Replace localhost With Private IP

In Prometheus config:

❌ WRONG:

targets: ['localhost:9100']


✅ CORRECT:

targets: ['172.31.10.170:9100']


📌 Because in cloud & Docker, localhost means inside container, not host machine.

⭐ If You Like This Project

Give it a ⭐ on GitHub 😄

