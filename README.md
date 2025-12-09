

# Container Monitoring Project using Prometheus & Grafana

## 📌 Project Overview

#### This project demonstrates how to monitor a containerized application using:

- Prometheus – Metrics collection

- cAdvisor – Container-level metrics (CPU, Memory, Network, Disk)

- Grafana – Visualization & dashboarding

- Alertmanager – Alerting based on thresholds

The setup uses Docker Compose to build a full end-to-end monitoring stack.

## 🚀 Architecture

The monitoring stack consists of:

- Prometheus scraping metrics from:

    - cAdvisor

   - Prometheus itself

   - Docker containers

- Grafana connected to Prometheus for dashboards

- Alertmanager triggering alerts

- Docker Compose orchestrating all services

      [Containers] → [cAdvisor] → [Prometheus] → [Grafana]
                             ↓
                        [Alertmanager]


## 🧩 Project Features

✔ Collect real-time metrics from Docker containers

✔ Pre-built dashboards in Grafana

✔ Custom Prometheus alerts

✔ Prometheus scrape configuration

✔ Infrastructure-as-code using Docker Compose


# 📂 Project Structure

      monitoring-project/
    │
    ├── docker-compose.yml
    │
    ├── prometheus/
    │   ├── prometheus.yml
    │   └── alert.rules.yml
    │
    └── grafana/
    └── provisioning/
        

## 🔧 Technologies Used

- Tool Purpose
- Docker	Containerization
- Docker Compose	Service orchestration
- Prometheus	Metrics collection
- Grafana	Visualization & dashboards
- cAdvisor	Container resource monitoring
- Alertmanager	Alerting


## 🛠️ Prerequisites

Ensure your system has:

- Docker

- Docker Compose

- Ports 9090, 3000, 9093, 8080 open


## 🚀 1. EC2 Instance Setup
Instance Details:

Instance Type: t2.micro / t3.micro

OS: Ubuntu 22.04

Storage: 8–20 GB

Security Group: Custom (opened ports manually)

### Security Group Configuration

The following inbound rules must be added:

| Port   | Service      | Purpose           |
| ------ | ------------ | ----------------- |
| 22     | SSH          | Connect to EC2    |
| 3000   | Grafana      | Dashboard Access  |
| 9090   | Prometheus   | Metrics UI        |
| 9093   | Alertmanager | Alerts interface  |
| 8080   | cAdvisor     | Container metrics |
| 80/443 | Optional     | Web apps          |

(Ensure that you restrict access using your IP or test IP ranges)

## 💻 2. Install Docker on EC2

login into your instance:

    ssh -i your-key.pem ubuntu@<EC2-PUBLIC-IP>
Run:

    sudo apt update
    sudo apt install -y docker.io
    sudo systemctl enable docker
    sudo systemctl start docker

Add your user to Docker group:

    sudo usermod -aG docker $USER
    newgrp docker

## 🐳 3. Install Docker Compose
    sudo apt install docker-compose -y

(or install the new plugin)

    sudo apt install docker-compose-plugin -y

Check version:

    docker compose version


## ▶️ How to Run the Project
###  Clone the Repository

     git clone https://github.com/<your-username>/monitoring-project.git
     cd monitoring-project

###  Start the Monitoring Stack

    docker-compose up -d

### Access Monitoring Tools via Browser

Replace <EC2-PUBLIC-IP> with your localhost.

| Service      | URL                                            |
| ------------ | ---------------------------------------------- |
| Prometheus   | [http://localhost:9090](http://localhost:9090) |
| Grafana      | [http://localhost:3000](http://localhost:3000) |
| cAdvisor     | [http://localhost:8080](http://localhost:8080) |
| Alertmanager | [http://localhost:9093](http://localhost:9093) |


## 🔐 Grafana Login

    Username: admin
    Password: admin

(You will be prompted to reset the password)

## 📊 Dashboards Included

- Container CPU usage

- Container Memory usage

- Network I/O

- Filesystem usage

- Prometheus internal metrics

- Custom alert panels

## ⚙️ Prometheus Configuration Example

    prometheus/prometheus.yml:

    global:
    scrape_interval: 15s

    scrape_configs:
      - job_name: 'prometheus'
        static_configs:
          - targets: ['prometheus:9090']

      - job_name: 'cadvisor'
        static_configs:
          - targets: ['cadvisor:8080']

## 🚨 Alert Example

prometheus/alert.rules.yml:

    groups:
    - name: container-alerts
       rules:
       - alert: HighContainerCPU
         expr: rate(container_cpu_usage_seconds_total[2m]) > 0.80
         for: 2m
         labels:
           severity: warning
         annotations:
           summary: "High CPU usage detected"

## 🧪 Testing Alerts

You can simulate load using:

    docker run --rm -it progrium/stress --cpu 4

## 📦 Stopping All Services

    docker-compose down


## 📚 Learning Outcomes

This project helped me understand:

✔ Setting up a complete monitoring stack

✔ Configuring Prometheus scrapers

✔ Creating dashboards in Grafana

✔ Writing alerting rules

✔ Docker networking & service orchestration

✔ Troubleshooting container metrics

## 💼 Use Cases

✔ DevOps monitoring stacks

✔ Cloud/On-prem infrastructure monitoring

✔ Performance observability

✔ Portfolio/Resume project

✔ Training lab for Prometheus & Grafana

## 🤝 Contributions

Pull requests are welcome. Feel free to fork the repository.

## 📧 Contact

Aniket Dnyaneshwar Kolhe

Role: Cloud/DevOps Engineer (Fresher)

Email: aiketkolhe1701@gmail.com

Location: Pune, Maharashtra






