# 🛒 Yolo E-commerce Project - Kubernetes (GKE) Deployment

This repository contains the production-grade Kubernetes deployment of the **Yolo E-commerce Web Application**, deployed to **Google Kubernetes Engine (GKE)**. The stack includes:

- **Frontend:** React application served from a Docker container
- **Backend:** Node.js Express server connected to a MongoDB database
- **Database:** MongoDB, deployed using a StatefulSet with persistent storage

---

## 🌐 Live Application URL

> 🚀 **Access the deployed app here:**  
> [http://34.136.127.98:3000/](http://34.136.127.98:3000/)


---


## ☁️ GKE Deployment Overview

- **Cluster Provisioning:** Done via `gcloud` CLI
- **Namespace:** `yolo-app`
- **Service Exposure:**
  - Frontend: `LoadBalancer`
  - Backend: `LoadBalancer` so that I can test the endpoints on postman 
  - MongoDB: `Headless Service`

---

## 📌 Key Kubernetes Concepts Applied

| Feature | Description |
|--------|-------------|
| **StatefulSet** | Used for MongoDB to maintain stable network identity & persistent volume |
| **Persistent Volume Claims** | Ensures MongoDB data survives pod restarts |
|
| **Controllers** | Deployments & StatefulSets ensure availability & self-healing |
| **Labels/Selectors** | Used for logical grouping and service discovery |
| **LoadBalancer** | Frontend exposed to public internet via LoadBalancer |
| **Resources** | CPU & Memory limits/requests defined for optimized usage |

---

## 🐋 Docker Images

- Frontend: [`docker.io/leilasinore/leila-yolo-frontend:v1.0.5`](https://hub.docker.com/)
- Backend: [`docker.io/leilasinore/leila-yolo-backend:v1.0.5`](https://hub.docker.com/)
- Mongodb:mongo:latest

Frontend and Backend images were pushed to Docker Hub and pulled in the Kubernetes manifests.

---
## ✅ Functional Verification

- ✅ **Users can add items to cart**  
- ✅ **Frontend communicates with backend**  
- ✅ **Backend connects to MongoDB StatefulSet**  
- ✅ **MongoDB data is retained after pod restarts**  
- ✅ **Resource limits and health checks help maintain app availability**  

## 👨‍💻 Author

**Name:** Leila    

## 📄 License

This project is licensed under the **MIT License**.





