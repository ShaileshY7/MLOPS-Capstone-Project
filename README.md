# 🎬 Movie Sentiment Analysis — End-to-End MLOps Project

An end-to-end **Machine Learning and MLOps project** that predicts whether a movie review expresses a **Positive** or **Negative** sentiment.

This project demonstrates the complete machine learning lifecycle, from **data ingestion and NLP preprocessing** to **model training, experiment tracking, data versioning, CI/CD, Docker containerization, AWS EKS deployment, and application monitoring with Prometheus and Grafana**.

---

## 📌 Project Overview

The objective of this project is to build a **Movie Sentiment Analysis** application that takes a movie review as input and predicts its sentiment.

### Example

**Input:**

```text
"This movie was absolutely amazing. The acting and story were excellent."
```

**Output:**

```text
Sentiment: Positive
```

Another example:

```text
"This movie was boring and the story was terrible."
```

**Output:**

```text
Sentiment: Negative
```

---

# 🏗️ Project Architecture

```text
                         MOVIE REVIEW
                              │
                              ▼
                    ┌───────────────────┐
                    │   Data Ingestion  │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Text Preprocessing│
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Feature Engineering│
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │  Model Training   │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Model Evaluation  │
                    └─────────┬─────────┘
                              │
                 ┌────────────┴────────────┐
                 ▼                         ▼
        ┌─────────────────┐       ┌─────────────────┐
        │ MLflow / DagsHub│       │      DVC        │
        │ Experiment      │       │ Data & Model    │
        │ Tracking        │       │ Versioning      │
        └─────────────────┘       └────────┬────────┘
                                           │
                                           ▼
                                     ┌────────────┐
                                     │  AWS S3    │
                                     └────────────┘

                              │
                              ▼
                       ┌─────────────┐
                       │  Flask API  │
                       └──────┬──────┘
                              │
                              ▼
                         ┌──────────┐
                         │  Docker  │
                         └────┬─────┘
                              │
                              ▼
                     ┌────────────────┐
                     │ GitHub Actions │
                     │     CI/CD      │
                     └───────┬────────┘
                             │
                             ▼
                     ┌────────────────┐
                     │   Amazon ECR   │
                     └───────┬────────┘
                             │
                             ▼
                     ┌────────────────┐
                     │   Amazon EKS   │
                     │   Kubernetes   │
                     └───────┬────────┘
                             │
                             ▼
                       ┌────────────┐
                       │LoadBalancer│
                       └─────┬──────┘
                             │
                    ┌────────┴─────────┐
                    ▼                  ▼
              ┌───────────┐      ┌───────────┐
              │Prometheus │─────►│  Grafana  │
              └───────────┘      └───────────┘
```

---

# 🛠️ Technology Stack

| Technology     | Purpose                    |
| -------------- | -------------------------- |
| Python 3.10    | Programming                |
| Conda          | Virtual environment        |
| Git            | Version control            |
| GitHub         | Source code repository     |
| Cookiecutter   | Project structure          |
| NLP            | Movie review processing    |
| MLflow         | Experiment tracking        |
| DagsHub        | ML experiment management   |
| DVC            | Data and model versioning  |
| AWS S3         | DVC remote storage         |
| Flask          | REST API                   |
| Docker         | Containerization           |
| GitHub Actions | CI/CD                      |
| Amazon ECR     | Docker image registry      |
| Amazon EKS     | Kubernetes deployment      |
| Kubernetes     | Container orchestration    |
| EC2            | Prometheus/Grafana servers |
| Prometheus     | Metrics collection         |
| Grafana        | Monitoring dashboards      |

---

# 📁 Project Structure

```text
MLOPS-Capstone-Project/
│
├── .dvc/
│
├── .github/
│   └── workflows/
│       └── ci.yaml
│
├── flask_app/
│   ├── app.py
│   ├── requirements.txt
│   └── ...
│
├── notebooks/
│   └── ...
│
├── scripts/
│   └── ...
│
├── src/
│   ├── logger/
│   │
│   ├── data_ingestion.py
│   ├── data_preprocessing.py
│   ├── feature_engineering.py
│   ├── model_building.py
│   ├── model_evaluation.py
│   └── register_model.py
│
├── tests/
│   └── ...
│
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
├── dvc.yaml
├── params.yaml
├── Dockerfile
├── requirements.txt
├── .gitignore
└── README.md
```

---

# 🔄 Machine Learning Pipeline

The machine learning pipeline is divided into multiple stages.

```text
Raw Movie Reviews
       │
       ▼
Data Ingestion
       │
       ▼
Data Preprocessing
       │
       ▼
Feature Engineering
       │
       ▼
Model Building
       │
       ▼
Model Evaluation
       │
       ▼
Model Registration
```

---

# 1️⃣ Project Setup

Clone the repository:

```powershell
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd MLOPS-Capstone-Project
```

Create the Conda environment:

```powershell
conda create -n atlas python=3.10
```

Activate it:

```powershell
conda activate atlas
```

Install Cookiecutter:

```powershell
pip install cookiecutter
```

Create the data science project structure:

```powershell
cookiecutter -c v1 https://github.com/drivendata/cookiecutter-data-science
```

Rename:

```text
src/models
```

to:

```text
src/model
```

---

# 2️⃣ MLflow + DagsHub

DagsHub is used with MLflow for experiment tracking.

Install:

```powershell
pip install dagshub mlflow
```

Connect the GitHub repository to DagsHub.

Configure the MLflow tracking URI provided by DagsHub.

The experiments can track:

* Model parameters
* Training metrics
* Validation metrics
* Model artifacts
* Experiment runs

Example:

```text
Experiment
    │
    ├── Parameters
    ├── Metrics
    ├── Model
    └── Artifacts
```

After running experiments:

```powershell
git add .
git commit -m "Add ML experiments"
git push origin main
```

---

# 3️⃣ DVC Setup

Initialize DVC:

```powershell
dvc init
```

Create a temporary local storage directory:

```text
local_s3/
```

Configure the local remote:

```powershell
dvc remote add -d mylocal local_s3
```

DVC is used for:

* Dataset versioning
* Model versioning
* Pipeline reproducibility
* Remote storage

---

# 4️⃣ NLP Data Processing

The input data consists of movie reviews.

The preprocessing stage prepares the raw text for machine learning.

Typical preprocessing operations can include:

```text
Raw Review
    ↓
Remove unnecessary characters
    ↓
Text normalization
    ↓
Remove unwanted words/noise
    ↓
Feature extraction
    ↓
Machine Learning Model
```

The exact preprocessing and feature-engineering methods depend on the implementation used in the project.

---

# 5️⃣ DVC Pipeline

Create:

```text
dvc.yaml
```

The pipeline contains stages for the ML workflow.

Create:

```text
params.yaml
```

for model and pipeline parameters.

Run the complete pipeline:

```powershell
dvc repro
```

Check the pipeline:

```powershell
dvc status
```

Track changes with Git:

```powershell
git add .
git commit -m "Add DVC pipeline"
git push origin main
```

---

# 6️⃣ AWS S3 as DVC Remote

Create an S3 bucket in AWS.

Create an IAM user with appropriate permissions.

Install DVC S3 support:

```powershell
pip install "dvc[s3]"
```

Install AWS CLI:

```powershell
pip install awscli
```

Configure AWS:

```powershell
aws configure
```

Add S3 as the DVC remote:

```powershell
dvc remote add -d myremote s3://<YOUR_BUCKET_NAME>
```

Push DVC data:

```powershell
dvc push
```

Check remote:

```powershell
dvc remote list
```

---

# 7️⃣ Flask REST API

The trained sentiment model is served using Flask.

Create:

```text
flask_app/
```

The Flask application accepts a movie review and returns a sentiment prediction.

Example API flow:

```text
Client
  │
  │ Movie Review
  ▼
Flask API
  │
  ▼
Trained ML Model
  │
  ▼
Prediction
  │
  ▼
Positive / Negative
```

Install Flask:

```powershell
pip install flask
```

Run locally:

```powershell
python flask_app/app.py
```

---

# 8️⃣ Generate Requirements

For the complete environment:

```powershell
pip freeze > requirements.txt
```

For the Flask application:

```powershell
cd flask_app
pipreqs . --force
cd ..
```

---

# 9️⃣ Testing

Create the test directory:

```text
tests/
```

Tests can cover:

* Data processing
* Feature generation
* Model prediction
* API response
* Input validation

Run tests:

```powershell
pytest
```

---

# 🔟 Dockerization

Create a `Dockerfile`.

Build the Docker image:

```powershell
docker build -t capstone-app:latest .
```

Run the application:

```powershell
docker run -p 8888:5000 capstone-app:latest
```

If the application requires a DagsHub authentication token:

```powershell
docker run -p 8888:5000 `
  -e CAPSTONE_TEST=<YOUR_TOKEN> `
  capstone-app:latest
```

The API will be available at:

```text
http://localhost:8888
```

---

# 1️⃣1️⃣ GitHub Actions CI/CD

Create:

```text
.github/workflows/ci.yaml
```

The CI/CD workflow automates:

```text
Git Push
   ↓
GitHub Actions
   ↓
Install Dependencies
   ↓
Run Tests
   ↓
Build Docker Image
   ↓
Authenticate with AWS
   ↓
Push Image to ECR
   ↓
Deploy to EKS
```

Required GitHub Secrets/Variables include:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
AWS_ACCOUNT_ID
ECR_REPOSITORY
CAPSTONE_TEST
```

**Never put actual credentials or tokens inside the repository.**

---

# 1️⃣2️⃣ Amazon ECR

Create an ECR repository:

```text
capstone-proj
```

The CI/CD pipeline builds the Docker image and pushes it to ECR.

The general workflow is:

```text
Docker Image
     │
     ▼
Amazon ECR
     │
     │
     ▼
Amazon EKS
```

---

# 1️⃣3️⃣ AWS EKS Deployment

Install and verify:

```powershell
aws --version
kubectl version --client
eksctl version
```

Create the EKS cluster:

```powershell
eksctl create cluster `
  --name flask-app-cluster `
  --region us-east-1 `
  --nodegroup-name flask-app-nodes `
  --node-type t3.small `
  --nodes 1 `
  --nodes-min 1 `
  --nodes-max 1 `
  --managed
```

Update Kubernetes configuration:

```powershell
aws eks --region us-east-1 update-kubeconfig `
  --name flask-app-cluster
```

Verify:

```powershell
aws eks list-clusters
```

Check cluster status:

```powershell
aws eks --region us-east-1 `
  describe-cluster `
  --name flask-app-cluster `
  --query "cluster.status"
```

Expected:

```text
"ACTIVE"
```

---

# 1️⃣4️⃣ Kubernetes

Check nodes:

```powershell
kubectl get nodes
```

Check namespaces:

```powershell
kubectl get namespaces
```

Check pods:

```powershell
kubectl get pods
```

Check services:

```powershell
kubectl get svc
```

Check deployments:

```powershell
kubectl get deployments
```

---

# 1️⃣5️⃣ Deploy Flask Application

The Kubernetes deployment uses the Docker image stored in Amazon ECR.

Example:

```text
Amazon ECR
     │
     │ Docker Image
     ▼
Kubernetes Deployment
     │
     ▼
Pod
     │
     ▼
Flask Application
```

The Kubernetes service exposes the application through an AWS LoadBalancer.

Check the service:

```powershell
kubectl get svc flask-app-service
```

Once the external endpoint is available, test:

```powershell
curl http://<LOAD_BALANCER_ENDPOINT>:5000/
```

---

# 1️⃣6️⃣ Prometheus Monitoring

Prometheus is deployed on an Ubuntu EC2 instance.

Example configuration:

```text
Instance Type: t3.medium
Storage: 20 GB
OS: Ubuntu
```

Required ports:

```text
22   → SSH
9090 → Prometheus
```

Update Ubuntu:

```bash
sudo apt update
sudo apt upgrade -y
```

Download Prometheus:

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.46.0/prometheus-2.46.0.linux-amd64.tar.gz
```

Extract:

```bash
tar -xvzf prometheus-2.46.0.linux-amd64.tar.gz
```

Move:

```bash
mv prometheus-2.46.0.linux-amd64 prometheus
sudo mv prometheus /etc/prometheus
sudo mv /etc/prometheus/prometheus /usr/local/bin/
```

---

# 1️⃣7️⃣ Prometheus Configuration

Edit:

```bash
sudo nano /etc/prometheus/prometheus.yml
```

Example:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "flask-app"
    static_configs:
      - targets:
          - "<YOUR_APPLICATION_ENDPOINT>:5000"
```

Verify:

```bash
cat /etc/prometheus/prometheus.yml
```

Run Prometheus:

```bash
/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml
```

Prometheus UI:

```text
http://<PROMETHEUS_EC2_PUBLIC_IP>:9090
```

---

# 1️⃣8️⃣ Grafana

Grafana is used to visualize metrics collected by Prometheus.

Launch an Ubuntu EC2 instance.

Recommended setup:

```text
Instance Type: t3.medium
Storage: 20 GB
OS: Ubuntu
```

Allow:

```text
22   → SSH
3000 → Grafana
```

Update:

```bash
sudo apt update
sudo apt upgrade -y
```

Download Grafana:

```bash
wget https://dl.grafana.com/oss/release/grafana_10.1.5_amd64.deb
```

Install:

```bash
sudo apt install ./grafana_10.1.5_amd64.deb -y
```

Start:

```bash
sudo systemctl start grafana-server
```

Enable:

```bash
sudo systemctl enable grafana-server
```

Check:

```bash
sudo systemctl status grafana-server
```

Open:

```text
http://<GRAFANA_EC2_PUBLIC_IP>:3000
```

---

# 1️⃣9️⃣ Connect Prometheus to Grafana

Inside Grafana:

```text
Connections
    ↓
Data Sources
    ↓
Add Data Source
    ↓
Prometheus
```

Prometheus URL:

```text
http://<PROMETHEUS_EC2_IP>:9090
```

Click:

```text
Save & Test
```

Grafana can then be used to create dashboards for application and infrastructure metrics.

---

# 📊 Monitoring Architecture

```text
                    ┌─────────────────┐
                    │   Flask API     │
                    │     EKS         │
                    └────────┬────────┘
                             │
                             │ Metrics
                             ▼
                    ┌─────────────────┐
                    │   Prometheus    │
                    │      EC2        │
                    └────────┬────────┘
                             │
                             │ PromQL
                             ▼
                    ┌─────────────────┐
                    │     Grafana     │
                    │      EC2        │
                    └─────────────────┘
```

---

# 🔐 Security

Do **not** commit credentials to GitHub.

Never commit:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
DAGSHUB_TOKEN
CAPSTONE_TEST
AWS_SESSION_TOKEN
Private SSH keys
```

Use:

* GitHub Secrets
* AWS IAM
* Environment variables
* Kubernetes Secrets
* AWS Secrets Manager
* IAM Roles

If a credential is accidentally pushed to GitHub, **revoke and rotate it immediately**.

---

# 💰 AWS Cost Management

This project uses AWS services that may generate charges.

During testing, monitor:

* EKS cluster
* EC2 instances
* EBS volumes
* Load Balancers
* ECR storage
* S3 storage
* NAT Gateway
* Elastic IPs
* CloudWatch

After testing, delete resources that are no longer required.

Delete EKS cluster:

```powershell
eksctl delete cluster `
  --name flask-app-cluster `
  --region us-east-1
```

Verify:

```powershell
eksctl get cluster --region us-east-1
```

Also check the AWS Billing and Cost Management dashboard.

---

# 🧹 Cleanup

Before finishing the project session:

```text
EKS Cluster       → Delete if no longer required
EC2 Prometheus    → Stop/terminate
EC2 Grafana       → Stop/terminate
Load Balancer     → Verify deleted with EKS
ECR               → Remove unused images
S3                → Remove unnecessary data
EBS               → Remove unused volumes
Elastic IP        → Release unused IPs
```

---

# 📚 Useful Commands

## Git

```powershell
git status
git add .
git commit -m "message"
git push origin main
```

## DVC

```powershell
dvc status
dvc repro
dvc push
dvc pull
dvc remote list
```

## Docker

```powershell
docker images
docker build -t capstone-app:latest .
docker run -p 8888:5000 capstone-app:latest
docker ps
docker stop <CONTAINER_ID>
```

## Kubernetes

```powershell
kubectl get nodes
kubectl get pods
kubectl get svc
kubectl get deployments
kubectl logs <POD_NAME>
kubectl describe pod <POD_NAME>
```

## AWS

```powershell
aws sts get-caller-identity
aws eks list-clusters
aws ecr describe-repositories
```

---

# 🎯 Complete Project Workflow

```text
                MOVIE REVIEW
                     │
                     ▼
              NLP PREPROCESSING
                     │
                     ▼
             FEATURE ENGINEERING
                     │
                     ▼
              MODEL TRAINING
                     │
                     ▼
             MODEL EVALUATION
                     │
              ┌──────┴──────┐
              │             │
              ▼             ▼
           MLflow          DVC
              │             │
              ▼             ▼
           DagsHub         S3
              │
              ▼
          MODEL REGISTER
              │
              ▼
           FLASK API
              │
              ▼
            DOCKER
              │
              ▼
       GITHUB ACTIONS
              │
              ▼
          AMAZON ECR
              │
              ▼
           AMAZON EKS
              │
              ▼
        LOAD BALANCER
              │
              ▼
         FLASK API
              │
        ┌─────┴─────┐
        ▼           ▼
   PROMETHEUS    APPLICATION
        │
        ▼
      GRAFANA
```

---

# 🚀 Future Improvements

The project can be extended with:

* Kubernetes Ingress
* HTTPS/TLS
* Helm charts
* Terraform
* Horizontal Pod Autoscaling
* Kubernetes resource limits
* Model drift detection
* Automated model retraining
* Automated integration testing
* Docker image vulnerability scanning
* Trivy security scanning
* Centralized logging
* ELK/OpenSearch
* AWS Secrets Manager
* IAM Roles for Service Accounts
* Canary deployments
* Blue/green deployments
* Automated monitoring alerts

---

# 🎓 Learning Outcomes

After completing this project, you gain practical experience in:

### Machine Learning

* Natural Language Processing
* Text preprocessing
* Feature engineering
* Sentiment classification
* Model training
* Model evaluation

### MLOps

* MLflow
* DagsHub
* DVC
* Data versioning
* Model versioning
* Reproducible ML pipelines

### DevOps

* Git
* GitHub Actions
* CI/CD
* Docker
* Kubernetes

### AWS

* IAM
* S3
* EC2
* ECR
* EKS
* Load Balancer
* AWS CLI

### Monitoring

* Prometheus
* PromQL
* Grafana
* Application monitoring
* Infrastructure monitoring

---

# 👨‍💻 Author

**Shailesh Yadav**

**Focus:** Machine Learning | MLOps | Python | Docker | Kubernetes | AWS | CI/CD

---

# ⭐ Project Summary

This project demonstrates how a **Movie Sentiment Analysis model** can be transformed from an experimental machine learning model into a deployable and monitored production application.

The complete lifecycle is:

```text
DATA
 ↓
NLP
 ↓
MODEL
 ↓
MLFLOW / DAGSHUB
 ↓
DVC / S3
 ↓
FLASK
 ↓
DOCKER
 ↓
GITHUB ACTIONS
 ↓
ECR
 ↓
EKS
 ↓
PROMETHEUS
 ↓
GRAFANA
```

**Movie Sentiment Analysis + MLOps = Complete End-to-End ML Deployment Pipeline**
