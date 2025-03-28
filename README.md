# ⚓️ WordPress Deployment on Kubernetes Cluster

This project demonstrates the deployment of a **WordPress application** with a **MySQL database** on a Kubernetes cluster using `kubeadm`. It covers the setup of namespaces, deployments, services, and ConfigMaps to ensure a smooth and secure deployment.

---

## 📚 Project Overview

### 1. ✅ Launching EC2 Instances
- Launched **3 EC2 instances** using `t2.medium` type.
- OS: **Ubuntu 22.04 LTS**

### 2. ⚡ Kubernetes Cluster Setup
- Set up the Kubernetes cluster using [`kubeadm`](https://github.com/Aareez01/kubernetes-v1.30.2-cluster-using-kubeadm).
- Followed steps to install Kubernetes v1.30.2 and configure the control plane and worker nodes.

---

## 🎯 Expected Outcomes
- ✅ Successful deployment of WordPress and MySQL on a Kubernetes cluster.
- ✅ Proper configuration of namespaces, deployments, services, and ConfigMaps.
- ✅ Secure and seamless communication between WordPress and MySQL using Kubernetes services.
- ✅ WordPress accessible via the public IP or domain on port 30080 using NodePort.
- ✅ Hands-on experience in managing containerized applications and using kubeadm for cluster setup.
- ✅ Done! WordPress is now successfully deployed on your Kubernetes cluster! 🎉
![Image2](/Deployed%20Website%20-2.png) 

---

## 🎯 Objective
To deploy a WordPress application connected to a MySQL database using Kubernetes, ensuring secure communication and scalability.

---

## ⚙️ Kubernetes Configuration

### 1. 🗂️ Create Namespace
```bash
kubectl create ns mywebsite
```

### 2. 🛢️ MySQL Deployment
Create a `mysql.yaml` file:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
  namespace: mywebsite
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: db
        image: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "redhat"
        - name: MYSQL_DATABASE
          value: "bigdata"
```

### 3. 🧩 MySQL Service (ClusterIP)
```bash
kubectl expose deployment -n mywebsite mysql --port 3306 --target-port 3306 --name=mysql-svc
```

---

### 4. 🌐 WordPress Deployment
Create a `wordpress.yaml` file:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
  namespace: mywebsite
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wp
        image: wordpress
        env:
        - name: WORDPRESS_DB_HOST
          value: "10.96.164.193"
        - name: WORDPRESS_DB_USER
          value: "root"
        - name: WORDPRESS_DB_PASSWORD
          value: "redhat"
        - name: WORDPRESS_DB_NAME
          value: "bigdata"
```

### 5. 🌐 WordPress Service (NodePort)
Create `wp-svc.yaml`:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: wp-svc
  namespace: mywebsite
spec:
  selector:
    app: wordpress
  type: NodePort
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
```

---

## ⚡ Access WordPress in Browser
```bash
http://<EC2_PUBLIC_IP>:30080
```

---

## 🛠️ ConfigMap for Environment Variables

### 1. 📄 Create MySQL ConfigMap
```bash
kubectl create configmap app-db --from-literal=MYSQL_ROOT_PASSWORD=redhat --from-literal=MYSQL_DATABASE=bigdata -n mywebsite
```

### 2. 📄 Create WordPress ConfigMap
```bash
kubectl create configmap app-wp -n mywebsite \
  --from-literal=WORDPRESS_DB_HOST=10.110.46.197 \
  --from-literal=WORDPRESS_DB_USER=root \
  --from-literal=WORDPRESS_DB_PASSWORD=redhat \
  --from-literal=WORDPRESS_DB_NAME=bigdata
```

---

## 🚀 Using ConfigMap in Deployment

Modify the environment section of your deployment file to reference the ConfigMap:
```yaml
envFrom:
  - configMapRef:
      name: app-db
  - configMapRef:
      name: app-wp
```

---

## 📡 Verifying Deployment
1. Check if all pods are running:
```bash
kubectl get pods -n mywebsite
```
2. Verify services:
```bash
kubectl get svc -n mywebsite
```
3. Access WordPress via browser:
```
http://<EC2_PUBLIC_IP>:30080
```

---

## 📝 Conclusion
- ✅ Successfully deployed a WordPress application with a MySQL backend on a Kubernetes cluster.  
- ✅ Used ConfigMaps to manage configuration, ensuring application portability and better management.

---
## 📢 Let's Connect!
- Stay updated on [LinkedIn](https://www.linkedin.com/in/-kartikjain/) for more DevOps projects and insights.
- Follow along as I explore **Cloud Infrastructure, Ansible Automation, and DevOps practices**.
- Let's collaborate and build scalable solutions together!

---
