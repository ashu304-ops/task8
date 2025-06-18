



# A complete DevOps pipeline to build, containerize, and deploy a static web game using modern CI/CD practices.

This project demonstrates:

*  Jenkins for Continuous Integration & Deployment
*  Ansible for Infrastructure Automation
* Docker for Containerization
* Kubernetes (via Minikube) for Orchestration

---

##  Tech Stack

| Layer          | Tooling               |
| -------------- | --------------------- |
| **Frontend**   | HTML, CSS, JavaScript |
| **CI/CD**      | Jenkins, Ansible, Git |
| **Containers** | Docker                |
| **Deployment** | Kubernetes (Minikube) |

---

##  Project Structure

```
simon-game/
├── ansible/
│   ├── inventory.ini
│   └── playbooks/
│       ├── install.yaml        # Install Docker, K8s, dependencies
│       ├── build.yaml          # Build Docker image
│       └── deploy.yaml         # Deploy to Kubernetes
├── simon-game/
│   ├── index.html
│   ├── style.css
│   ├── index.js
│   ├── sounds/                 # Audio files
│   └── Dockerfile              # For static site container
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── jenkins/
│   ├── Jenkinsfile             # CI/CD pipeline definition
│   └── Dockerfile              # Optional Jenkins image setup
```

---

## ⚙️ CI/CD Pipeline Flow

```
Git Push → Jenkins → Ansible → Docker → Kubernetes → Live App
```

### Pipeline Stages

1.  Jenkins detects push and triggers pipeline
2.  Ansible installs Docker/K8s (optional)
3.  Ansible builds Docker image of the static site
4.  Ansible deploys app using K8s manifests
5.  App becomes accessible via Minikube service

---

##  Deployment Guide

### 1. Clone the Repository

```bash
git clone https://github.com/DaChe01/Full-DevOps-Workflow.git
cd Full-DevOps-Workflow/simon-game
```

### 2. Jenkins Setup

* Open Jenkins UI → New Item → **Pipeline**
* Use `jenkins/Jenkinsfile` (move to root if preferred)
* Configure Git repo and trigger build

### 3. Jenkins Agent Configuration

Optional: Build a Jenkins agent with necessary tools pre-installed (Ansible, SSH, etc.)

```bash
cd jenkins/
docker build -t custom-jenkins -f Dockerfile .
docker run -p 8080:8080 -v jenkins_home:/var/jenkins_home custom-jenkins
```

Or manually install:

```bash
sudo apt install ansible openssh-client rsync -y
```

### 4. Minikube & Kubernetes Setup

Start Minikube and ensure `kubectl` is pointing to it:

```bash
minikube start
kubectl config use-context minikube
```

### 5. Access the Deployed App

#### Via Minikube (Auto-expose service):

```bash
minikube service simon-game-service
```

#### Or Port Forward for Remote Access:

```bash
kubectl port-forward service/simon-game-service 9090:80
```

Then on your local machine:

```bash
ssh -L 9090:localhost:9090 your-user@your-server-ip
```

Open [http://localhost:9090](http://localhost:9090) in your browser.

---

## Jenkinsfile Overview

Key Stages:

* **Setup**: Runs `install.yaml` via Ansible (optional)
* **Build**: Executes `build.yaml` to build Docker image
* **Deploy**: Applies K8s manifests with `deploy.yaml`

You can skip the setup step if the system already has Docker, kubectl, and Minikube configured.

---

## Notes

* Ensure SSH access from Jenkins to the deployment server.
* Passwordless SSH and correct Ansible inventory setup are essential.
* Minikube is used for demonstration; adapt for full-scale K8s clusters as needed.

---

##  Future Enhancements

* Integrate with cloud Kubernetes services (e.g., EKS, GKE)
* Add GitHub Webhooks for auto-triggering builds
* Setup monitoring and logging (e.g., Prometheus, ELK stack)

---
