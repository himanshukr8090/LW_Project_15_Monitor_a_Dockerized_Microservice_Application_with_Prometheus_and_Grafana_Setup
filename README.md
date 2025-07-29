# 🚀 Project 10: Monitoring Dockerized Microservices with Prometheus & Grafana via Ansible

This project demonstrates how to set up a monitoring stack using **Prometheus**, **Node Exporter**, and **Grafana**, all deployed inside **Docker containers** via **Ansible**. All Docker images are built **manually** (not pulled from Docker Hub), and container management is fully automated with Ansible.

---

## 🧰 Tech Stack

- 🐧 OS: Ubuntu 22.04 (Base Image)
- 🐳 Docker (manual image build)
- ⚙️ Ansible
- 📊 Prometheus
- 📈 Grafana
- 📦 Node Exporter

---

## 📁 Project Structure

```
project_10/
├── README.md
├── ansible-monitoring/
│   ├── inventory.ini
│   └── site.yml
└── docker/
    ├── grafana/
    │   └── Dockerfile
    ├── node_exporter/
    │   └── Dockerfile
    └── prometheus/
        ├── Dockerfile
        └── prometheus.yml
```

---

## ⚙️ Step-by-Step Setup

### 1️⃣ Ansible Inventory File

**File:** `ansible-monitoring/inventory.ini`

```ini
[monitoring]
localhost ansible_connection=local
```

---

### 2️⃣ Ansible Playbook to Deploy Monitoring Stack

**File:** `ansible-monitoring/site.yml`

```yaml
- name: Deploy Monitoring Stack
  hosts: monitoring
  become: true
  tasks:

    - name: Build Prometheus Docker image
      docker_image:
        name: prometheus_manual1
        source: build
        build:
          path: ../docker/prometheus

    - name: Run Prometheus container
      docker_container:
        name: prometheus
        image: prometheus_manual1
        network_mode: host
        restart_policy: always
        state: started

    - name: Build Node Exporter Docker image
      docker_image:
        name: node_exporter_manual
        source: build
        build:
          path: ../docker/node_exporter

    - name: Run Node Exporter container 1
      docker_container:
        name: node_exporter_1
        image: node_exporter_manual
        network_mode: host
        restart_policy: always
        state: started
        command: "/opt/node_exporter/node_exporter --web.listen-address=:9100"

    - name: Run Node Exporter container 2
      docker_container:
        name: node_exporter_2
        image: node_exporter_manual
        network_mode: host
        restart_policy: always
        state: started
        command: "/opt/node_exporter/node_exporter --web.listen-address=:9101"

    - name: Build Grafana Docker image
      docker_image:
        name: grafana_manual
        source: build
        build:
          path: ../docker/grafana

    - name: Run Grafana container
      docker_container:
        name: grafana
        image: grafana_manual
        network_mode: host
        restart_policy: always
        state: started
```

---

## 🐳 Dockerfiles

### 🔹 Prometheus

**File:** `docker/prometheus/Dockerfile`

```dockerfile
FROM ubuntu:22.04

RUN apt-get update && apt-get install -y wget tar && \
    wget https://github.com/prometheus/prometheus/releases/download/v2.51.1/prometheus-2.51.1.linux-amd64.tar.gz && \
    tar -xvf prometheus-2.51.1.linux-amd64.tar.gz && \
    mv prometheus-2.51.1.linux-amd64 /opt/prometheus && \
    rm prometheus-2.51.1.linux-amd64.tar.gz

COPY prometheus.yml /opt/prometheus/prometheus.yml

EXPOSE 9090

CMD ["/opt/prometheus/prometheus", "--config.file=/opt/prometheus/prometheus.yml"]
```

**Prometheus Config:** `docker/prometheus/prometheus.yml`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node_exporters'
    static_configs:
      - targets:
          - 'localhost:9100'
          - 'localhost:9101'
```

---

### 🔹 Node Exporter

**File:** `docker/node_exporter/Dockerfile`

```dockerfile
FROM ubuntu:22.04

RUN apt update && apt install -y wget tar && \
    wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz && \
    tar -xvf node_exporter-*.tar.gz && mv node_exporter-1.8.1.linux-amd64 /opt/node_exporter

EXPOSE 9100

CMD ["/opt/node_exporter/node_exporter"]
```

---

### 🔹 Grafana

**File:** `docker/grafana/Dockerfile`

```dockerfile
FROM ubuntu:22.04

RUN apt update && apt install -y wget software-properties-common apt-transport-https gnupg2 && \
    wget -q -O - https://packages.grafana.com/gpg.key | apt-key add - && \
    add-apt-repository "deb https://packages.grafana.com/oss/deb stable main" && \
    apt update && apt install -y grafana

EXPOSE 3000

CMD ["/usr/sbin/grafana-server", "--homepath=/usr/share/grafana"]
```

---

## ▶️ How to Run

1. **Install Dependencies**
   ```bash
   sudo apt install docker.io ansible
   ```

2. **Run Ansible Playbook**
   ```bash
   cd ansible-monitoring
   ansible-playbook -i inventory.ini site.yml
   ```

3. **Access Web UIs:**
   - Prometheus: [http://localhost:9090](http://localhost:9090)
   - Grafana: [http://localhost:3000](http://localhost:3000)
     - Default Login: `admin` / `admin`

---

## 📌 What You Learned

- Build Prometheus, Node Exporter, and Grafana from scratch (no base images)
- Use Ansible to automate Docker builds and deployments
- Monitor multiple Node Exporters from Prometheus
- Visualize metrics via Grafana

---

## 🙌 Author

**Himanshu Kumar Singh**  
B.Tech CSE | DevOps & Cloud Enthusiast  
🔗 [LinkedIn](https://www.linkedin.com/in/himanshukrsingh0)

