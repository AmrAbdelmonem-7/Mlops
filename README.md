# California Housing Price Prediction (MLOps Pipeline)

This project demonstrates an end-to-end MLOps workflow covering model training, experiment tracking, model registry, containerization, and orchestration on Kubernetes.

##  Project Overview
- **Lab 1:** Tracked XGBoost experiments using MLflow, evaluated runs based on RMSE, and registered the best model (`California_Housing_Predictor`) with the alias `@staging`.
- **Lab 2:** Packaged the trained model into a FastAPI application, containerized it using Docker, and deployed it on a local Kubernetes cluster using Minikube.

---

## 🛠️ Tech Stack
- **Machine Learning:** XGBoost, Scikit-Learn
- **Experiment Tracking:** MLflow
- **API Framework:** FastAPI, Uvicorn
- **Containerization:** Docker
- **Orchestration:** Kubernetes (Minikube)

---

##  Repository Structure
- `train.py`: Trains the XGBoost model and logs metrics/artifacts to MLflow.
- `export_model.py`: Downloads the registered `@staging` model artifact locally.
- `app.py`: FastAPI application serving predictions via HTTP endpoints (`/health`, `/predict`).
- `requirements.txt`: Python dependencies.
- `Dockerfile`: Container definition for building the API image.
- `k8s-deployment.yaml`: Kubernetes Deployment and LoadBalancer Service configurations.
- `Task37.pdf`: Full step-by-step documentation with screenshots.

---

##  How to Run Locally

### 1. Model Tracking (MLflow)
```bash
# Start MLflow server
mlflow server --backend-store-uri sqlite:///mlruns.db --default-artifact-root ./artifacts --host 127.0.0.1 --port 5000
Export & Deploy (Kubernetes on Minikube)
Bash
# Export the staging model
python export_model.py

# Start Minikube & build image
minikube start --driver=docker
minikube image build -t california-housing-api:v1 .

# Deploy to Kubernetes
kubectl apply -f k8s-deployment.yaml

# Access the service
minikube service housing-api-service
PI Endpoints
GET /health: Health check endpoint verifying model load status.

POST /predict: Generates housing price predictions based on input features.

Interactive docs available at: /docs

# Run training
python train.py
