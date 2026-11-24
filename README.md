# 🌐 Visitor Counter — Flask + Redis on Docker & Kubernetes

> **Cloud Computing Final Project**  
> Technologies: **Python Flask · Redis · Docker · Kubernetes · HPA · Hybrid Cloud Failover · Cost Optimization**

---

## 🚀 Overview

A production-ready, distributed **Visitor Counter** microservice built with Flask and Redis, containerised with Docker and deployed on Kubernetes. The project demonstrates:

- ✅ Containerisation with Docker & Docker Compose
- ✅ Kubernetes deployment with health probes
- ✅ Horizontal Pod Autoscaler (HPA) for elastic scaling
- ✅ Hybrid Cloud failover with NGINX reverse-proxy
- ✅ Cost optimisation (87.6% image size reduction, 52.8% compute savings)

---

## 📁 Repository Structure

```
visitor-counter/
│
├── visitor-counter-k8s-clou-main/     # Main project source
│   ├── app/                           # Flask application & Docker assets
│   │   ├── app.py                     # Main Flask app (counter, health, CPU loader)
│   │   ├── requirements.txt           # Python dependencies
│   │   ├── Dockerfile                 # Baseline Docker image
│   │   ├── Dockerfile.optimized       # Multi-stage slim image (cost optimised)
│   │   ├── docker-compose.yml         # Local container orchestration
│   │   └── templates/index.html       # Responsive dashboard UI
│   │
│   ├── k8s/                           # Kubernetes manifests
│   │   ├── configmap.yaml
│   │   ├── redis-deployment.yaml
│   │   ├── redis-service.yaml
│   │   ├── flask-deployment.yaml
│   │   ├── flask-service.yaml
│   │   └── hpa.yaml                   # Horizontal Pod Autoscaler
│   │
│   ├── failover/                      # Hybrid Cloud Failover assets
│   │   ├── cloud_standby.py           # DR standby microservice (port 5001)
│   │   ├── nginx-failover.conf        # NGINX health-check proxy config
│   │   └── simulate_failover.py       # Automated failover monitor
│   │
│   ├── cost_optimization/
│   │   └── cost_analysis_report.md    # Before/after metrics & savings
│   │
│   ├── scripts/                       # One-click PowerShell scripts
│   │   ├── 1_docker_test.ps1
│   │   ├── 2_k8s_deploy.ps1
│   │   ├── 3_failover_test.ps1
│   │   ├── 4_benchmark_costs.ps1
│   │   └── load_test.py               # Multi-threaded HPA load tester
│   │
│   └── docs/
│       ├── Project_Report.md
│       ├── SCREENSHOTS_GUIDE.md
│       └── COMPLETE_EXECUTION_AND_SCREENSHOTS_GUIDE.md
│
└── screenshots/                       # Project demo screenshots
```

---

## ⚡ Quick Start

### Prerequisites
- Docker Desktop (with Kubernetes enabled, or Minikube)
- `kubectl` CLI
- Python 3.10+

---

### Task A — Run with Docker Compose

```powershell
cd visitor-counter-k8s-clou-main\app
docker compose up -d --build
```

| Endpoint | URL |
|---|---|
| App | http://localhost:5000 |
| Health Check | http://localhost:5000/health |

---

### Task B — Deploy to Kubernetes

```powershell
cd visitor-counter-k8s-clou-main\scripts
.\2_k8s_deploy.ps1
```

Or manually apply manifests:

```powershell
cd visitor-counter-k8s-clou-main\k8s
kubectl apply -f configmap.yaml
kubectl apply -f redis-deployment.yaml
kubectl apply -f redis-service.yaml
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
```

Access via: `http://localhost:30080`

---

### Task C — HPA Autoscaling

```powershell
# Enable metrics server
minikube addons enable metrics-server

# Watch HPA scale in real time
kubectl get hpa flask-hpa -w

# Generate load (15 threads, 60 seconds)
python scripts\load_test.py 15 60
```

Pods scale from **2 → 6 replicas** as CPU exceeds the 50% threshold.

---

### Task D — Hybrid Cloud Failover

```powershell
# Start failover monitor
.\scripts\3_failover_test.ps1

# In another terminal, simulate primary failure
kubectl scale deployment flask-deployment --replicas=0

# Restore primary
kubectl scale deployment flask-deployment --replicas=2
```

NGINX automatically routes traffic to the cloud DR standby with zero 502 errors.

---

### Task E — Cost Optimisation Results

| Technique | Baseline | Optimised | Saving |
|---|---|---|---|
| Multi-Stage Slim Build | 1,020 MB | 126 MB | **87.6% image size ↓** |
| Pod Resource Right-Sizing | 500m CPU / 512Mi RAM | 50m CPU / 64Mi RAM | **6× pod density ↑** |
| Dynamic HPA | 4,320 pod-hrs/mo | 2,040 pod-hrs/mo | **52.8% compute ↓** |

Full report: [`cost_optimization/cost_analysis_report.md`](visitor-counter-k8s-clou-main/visitor-counter-k8s-clou-main/cost_optimization/cost_analysis_report.md)

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Application | Python 3.10, Flask, Gunicorn |
| Cache / Store | Redis |
| Containerisation | Docker, Docker Compose |
| Orchestration | Kubernetes, HPA |
| Load Balancing | NGINX |
| Automation | PowerShell, Python |

---

## 📄 License

This project is submitted as part of a Cloud Computing course. All rights reserved © NiteshRajSingh796.
