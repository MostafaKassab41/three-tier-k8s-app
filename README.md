# Three-Tier Application with Kubernetes & Helm

A **three-tier web application** deployed with **Docker Compose** (for local development) and **Kubernetes with Helm** (for production).  
The stack includes:

- **Frontend**: React application (served on port `3000`)  
- **Backend**: Node.js / Express API (served on port `3500`)  
- **Database**: MongoDB (served on port `27017`)  

[Secure To-Do List — Three-Tier Application](./Application-Code/README.md#secure-to-do-list---three-tier-application)

This project demonstrates how to **containerize applications**, manage them with **Docker Compose**, and deploy them to a **Kubernetes cluster** using **Helm**, including configuration via **ConfigMaps, Secrets, and Ingress**.

---

## 🚀 Features

- ✅ Containerized frontend, backend, and database  
- ✅ Environment variable management via ConfigMaps & Secrets  
- ✅ Kubernetes manifests templated with Helm  
- ✅ Health checks with liveness & readiness probes  
- ✅ Rolling updates & resource requests/limits  
- ✅ Ingress routing (frontend + optional backend APIs)  
- ✅ Easily configurable for **dev**, **staging**, and **production** environments  

---

## 🏗️ Architecture

### Kubernetes Deployment
![Kubernetes Architecture](./docs/k8s-architecture.png)


---

## 📂 Project Structure

```
Application-Code/
│    ├── backend/
│    ├── frontend/
│    ├── docker-compose.yml 
│    ├── Dockerfile.backend 
│    ├── Dockerfile.frontend 
│    └── README.md
│ 
├── helm/ # Helm chart for Kubernetes
│ ├── Chart.yaml 
│ ├── values.yaml 
│ └── templates/ # K8s manifests (frontend, backend, db, ingress)
│ 
├── k8s_manual_Test/ # Raw Kubernetes YAMLs for testing (if not using Helm)
└── README.md
```


---

## 🐳 Run Locally with Docker Compose

[Run Locally with Docker Compose](./Application-Code/README.md#run-locally-with-docker-compose)


---

## 🚀 Deploy to Kubernetes with Helm

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/MostafaKassab41/three-tier-k8s-app.git
cd three-tier-k8s-app
```

### 2️⃣ Start Minikube & Enable Ingress
```bash
minikube start
minikube addons enable ingress
```

### 3️⃣ Install Helm
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

### 4️⃣ Deploy with Helm
```bash
helm install devops-todo helm/ --namespace test --create-namespace
```

### 5️⃣ Add Host to /etc/hosts
Assign vertual subdomain for every frontend and backend service
```bash
echo "$(minikube ip) app.devops-todo.com api.devops-todo.com" | sudo tee -a /etc/hosts
```

### 6️⃣ Access App

**Frontend** → http://app.devops-todo.com   

**Backend** → http://api.devops-todo.com/api/tasks  # List tasks
            → http://api.devops-todo.com/ok     # Health check

###  7️⃣ Cleanup
```bash
helm uninstall devops-todo --namespace test
```


---

## 🔐 Secrets & Configs

- Database Credentials → stored in Secrets
- Backend Connection String → injected as env vars
- Frontend Backend URL → stored in ConfigMap
```
MONGO_USERNAME: from Secret
MONGO_PASSWORD: from Secret
REACT_APP_BACKEND_URL: from ConfigMap
```


---

## ⚙️ Customization

### Edit `helm/values.yaml` to set custom values
- Replica counts
- Container images & tags
- Resource requests/limits
- Ingress hostname & exposure


---

## ✅ Health Checks

- **Liveness probes** ensure containers restart if stuck
- **Readiness probes** ensure traffic only hits ready pods


---

## 📝 License

[MIT License](./LICENSE)