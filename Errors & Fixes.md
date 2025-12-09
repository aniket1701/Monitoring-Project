
## A collection of real DevOps errors I encountered while working with AWS, Docker, Prometheus, Grafana, CI/CD etc., along with step-by-step solutions.


## 🚨 Permission denied: /var/run/docker.sock

#### ❌ Error Log:
PermissionError: [Errno 13] Permission denied: '/var/run/docker.sock'

#### 🔍 Root Cause:
Your user is not added to the docker group, so Docker Engine rejects access.

#### ✅ Solution:
    sudo usermod -aG docker $USER  
    newgrp docker  

#### 🧠 Notes:
Always re-login after adding docker permissions.

&nbsp;

## 🚨 Prometheus failed due to wrong mounting of prometheus.yml file 

#### 🔍 Root Cause:
Path of permissions.yml file 

#### ✅ Solution:
you have to change the path as per your location of Prometheus.yml file in Docker-compose.yml.

#### 🧠 Notes:
Always Remember to change the Path.

&nbsp;

## 🚨 Docker Compose is failing with -> KeyError: 'ContainerConfig'.

#### 🔍 Root Cause:
It happens when a container exists but is corrupted or has no config.

####✅ Solution:

### Step 1 — Remove all old/stopped containers for Prometheus & Alertmanager

Run:

    docker rm -f monitoring-project_prometheus_1 monitoring-project_alertmanager_1


Also remove leftover anonymous containers:

    docker ps -a | grep monitoring-project


Remove all of them:

    docker rm -f <container-id>


### Step 2 — Remove broken images (they cause missing ContainerConfig)
    docker rmi prom/prometheus:latest
    docker rmi prom/alertmanager:latest


If Docker says “in use”, run:

    docker rmi -f prom/prometheus:latest
    docker rmi -f prom/alertmanager:latest

### Step 3 — Recreate everything cleanly

Run:

    docker-compose down --volumes --remove-orphans
    docker-compose pull
    docker-compose up -d

## ⚠️ If the error still happens: UPGRADE DOCKER-COMPOSE



Install the new plugin version:

    sudo apt remove docker-compose
    sudo apt install docker-compose-plugin


Check version:

    docker compose version


Now run:

    docker compose up -d


#### 🧠 Notes:
Always check for Running container before practicing and if any container is running on same port that we are going to practice kill that container.


&nbsp;


## 🚨 Common Error: Inbound Rules Not Working on EC2
| Error | Description | Fix |
|-------|-------------|-----|
| Port not reachable | Browser cannot open Prometheus or Grafana | Add inbound rules in EC2 Security Group |

### ✅ Solution:
Add the following inbound rules to your EC2 Instance:

| Port | Description | Purpose |
|------|-------------|---------|
| 3000 | Grafana | Dashboard UI |
| 9090 | Prometheus | Metrics UI |
| 8080 | cAdvisor | Container metrics |
| 9093 | Alertmanager | Alerts UI |

Source: `0.0.0.0/0` (only for testing — restrict in production)

### 🧠 Note:
If the rules are added but still not working, restart Docker:


    docker-compose down
    docker-compose up -d


## Thank you..!!!
