# 📘 Explanation: YOLO E-Commerce Kubernetes Deployment

This document explains the key architectural and deployment decisions made during the Kubernetes deployment of the YOLO e-commerce application to Google Kubernetes Engine (GKE).

---

## 🚀 Choice of Kubernetes Objects

### ✅ Deployments (Frontend and Backend)
- I used **Deployment** resources to manage the **frontend** and **backend** components of the application.
- Deployments are ideal for stateless applications, offering rolling updates, version control, and automatic rescheduling of failed pods.
- Each Deployment is configured with proper resource limits (`cpu` and `memory`) to ensure cost-efficient and stable performance, especially under GCP’s Free Tier.

### ✅ StatefulSet (MongoDB)
- A **StatefulSet** was used to deploy **MongoDB**, which is a stateful service requiring persistent identity and stable storage.
- Unlike Deployments, StatefulSets provide consistent network identities (e.g., `mongodb-0`) and predictable persistent storage across pod restarts.
- This is essential for MongoDB, which needs stability in storage and network configuration to maintain data integrity and connection reliability.

---

## 🌐 Pod Exposure: Services


### ✅ Frontend - LoadBalancer Service
- The **frontend** is exposed via a `LoadBalancer` service.
- This creates an external IP through which users can access the application from the internet.
- This method integrates well with GKE, as it automatically provisions a GCP load balancer and maps traffic to the frontend pods.

### ✅ Backend - LoadBalancer Service
- The **backend** is exposed externally via a `LoadBalancer` service.
- This makes it reachable externally, allowing for testing in platforms such as postman which is the case in real world applications.Security is not an issue because real world apis are wrapped with authentication apis which will not allow access unless someone is authenticated so that is why I used Loadbalancer service instead of clusterIP.

---

## 💾 Use of Persistent Storage

### ✅ MongoDB Storage
- MongoDB is backed by a **PersistentVolumeClaim (PVC)**, which ensures that data is retained even when the pod is deleted or restarted.
- The storage is mounted at `/data/db`, MongoDB’s default data directory.
- This guarantees that cart data and other database entries persist across sessions and deployments.

### ❌ Frontend and Backend Storage
- Persistent storage was not configured for the frontend and backend services, as they are **stateless** by design.
- All business logic and UI assets are rebuilt on container image updates, and no runtime data is stored locally.

---

## 📌 Summary

| Component | K8s Object | Storage Type | Exposure Method |
|----------|------------|--------------|-----------------|
| Frontend | Deployment | Ephemeral    | LoadBalancer    |
| Backend  | Deployment | Ephemeral    | LoadBalancer       |
| MongoDB  | StatefulSet| Persistent   | ClusterIP       |

These architectural decisions ensure that the YOLO application is:
- Scalable and modular,
- Secure by limiting exposure,
- Durable through use of persistent volumes for stateful data,
- And optimized for cloud-native environments like GKE.

---
