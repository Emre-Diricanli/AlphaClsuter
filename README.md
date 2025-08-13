# 🏠 AlphaCluster — My Homelab Infrastructure

![Proxmox](https://img.shields.io/badge/Proxmox-Cluster-orange?style=for-the-badge&logo=proxmox)
![Docker](https://img.shields.io/badge/Docker-Containers-blue?style=for-the-badge&logo=docker)
![Ansible](https://img.shields.io/badge/Ansible-Automation-black?style=for-the-badge&logo=ansible)
![Grafana](https://img.shields.io/badge/Grafana-Monitoring-orange?style=for-the-badge&logo=grafana)
![Prometheus](https://img.shields.io/badge/Prometheus-Metrics-orange?style=for-the-badge&logo=prometheus)
![Pi-hole](https://img.shields.io/badge/Pi--hole-Ads%20Blocked-brightgreen?style=for-the-badge&logo=pi-hole)

---

## 📜 Overview

**AlphaCluster** is my personal **4-node Proxmox** homelab designed for:
- **Containerized microservices** in Docker (inside LXC containers for isolation)
- **Automation** with Ansible
- **Monitoring** with Prometheus + Grafana
- **Secure remote access** via Twingate
- **Reverse proxy & SSL management** via NGINX Proxy Manager

**Subnet:** `192.168.86.0/24`

---

## 🖥️ Hardware

| Node   | Model                | CPU Cores | RAM   | Storage | Notes |
|--------|----------------------|-----------|-------|---------|-------|
| Master | HP Z2 Mini G3         | 4 cores   | 16 GB | SSD     | Main orchestration node |
| Node1  | HP Z2 Mini G3         | 4 cores   | 16 GB | SSD     | Workflow automation |
| Node2  | HP Z2 Mini G3         | 4 cores   | 16 GB | SSD     | Networking & reverse proxy |
| Node3  | HP Z2 Mini G3         | 4 cores   | 16 GB | SSD     | Monitoring & uptime checks |

---

## 📦 Service Deployment

### **Master Node**
| LXC Container              | IP               | Service(s)                 |
|----------------------------|------------------|----------------------------|
| `ansible-ctl-master`       | 192.168.86.200   | Ansible control node       |
| `twingate-prod-master`     | 192.168.86.201   | VPN (Twingate)             |
| `vault-prod-master`        | 192.168.86.202   | HashiCorp Vault            |
| `pihole-prod-master`       | 192.168.86.203   | Pi-hole (DNS / Ad-block)   |
| `nginxproxy-dev-master`    | 192.168.86.204   | NGINX Proxy Manager        |
| `portainer-dev-master`     | 192.168.86.205   | Portainer (Docker GUI)     |
| `dashboard-dev-master`     | 192.168.86.206   | Dashy (Service Dashboard)  |

---

### **Node1**
| LXC Container              | IP               | Service(s) |
|----------------------------|------------------|------------|
| `n8n-dev-node1`            | 192.168.86.211   | n8n (Automation workflows) |

---

### **Node2**
| LXC Container              | IP               | Service(s) |
|----------------------------|------------------|------------|
| `traefik-dev-node2`        | 192.168.86.221   | Traefik (Experimental) |

---

### **Node3**
| LXC Container              | IP               | Service(s)       |
|----------------------------|------------------|------------------|
| `prometheus-prod-node3`    | 192.168.86.231   | Prometheus       |
| `grafana-prod-node3`       | 192.168.86.232   | Grafana          |
| `uptime-dev-node3`         | 192.168.86.233   | Uptime Kuma      |

---

## 🛠️ Automation & Monitoring

### **Ansible**
- Full **inventory management** for all LXCs
- Automated **updates**, **docker deployments**, and **config changes**
- Playbooks stored in private repo

### **Monitoring Stack**
- **Prometheus** scrapes metrics from:
  - Nodes
  - LXC containers
  - Docker services
- **Grafana** dashboards for:
  - System health
  - Service uptime
  - Network usage

---

## 🌐 Networking

- **LAN Subnet:** `192.168.86.0/24`
- **Remote Access:** Twingate VPN
- **Reverse Proxy:** NGINX Proxy Manager (internal domains: `*.home`)
- **DNS Filtering:** Pi-hole

---

## 🗺️ Topology Diagram

```mermaid
graph TD
    subgraph Master
        A1[Ansible] -->|Deploys| A2[Docker Services]
        A3[Twingate] -->|VPN Access| Net
        A4[Vault] --> Sec
        A5[Pi-hole] --> DNS
        A6[NGINX Proxy Manager] --> RP
        A7[Portainer] --> Docker
        A8[Dashy] --> UI
    end

    subgraph Node1
        B1[n8n] --> Automations
    end

    subgraph Node2
        C1[Traefik] --> Routing
    end

    subgraph Node3
        D1[Prometheus] --> Metrics
        D2[Grafana] --> Dashboards
        D3[Uptime Kuma] --> Monitoring
    end