# Docker : From Basics to Advanced 

## 📦 Docker Basics

### 🔹 Basic Docker Commands

| Command                                 | Description                             |
| --------------------------------------- | --------------------------------------- |
| `docker --version`                      | Show Docker version installed           |
| `docker pull <image>`                   | Download image from Docker Hub          |
| `docker images`                         | List local images                       |
| `docker run <image>`                    | Run container from image                |
| `docker ps`                             | Show running containers                 |
| `docker ps -a`                          | Show all containers (running + stopped) |
| `docker stop <container_id>`            | Stop a container                        |
| `docker rm <container_id>`              | Remove a container                      |
| `docker rmi <image_id>`                 | Remove an image                         |
| `docker exec -it <container> /bin/bash` | Get interactive terminal in a container |

---

## 🐳 Docker Intermediate Concepts

### 🔹 Dockerfile Example

```Dockerfile
# Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

### 🔹 Build and Run

```bash
docker build -t mynodeapp .
docker run -d -p 3000:3000 mynodeapp
```

---

## 📁 Docker Volume Mounting

### 🔹 Bind Mount

```bash
docker run -v $(pwd)/data:/app/data mynodeapp
```

### 🔹 Named Volume

```bash
docker volume create mydata
docker run -v mydata:/app/data mynodeapp
```

### 🔹 Anonymous Volume

```bash
docker run -v /app/data mynodeapp
```

### 🔹 Inspect Volumes

```bash
docker volume ls
docker volume inspect mydata
```

### 📊 Diagram: Docker Volume

```
[Host] ──/data──▶ [Named/Bind Volume] ──▶ [Container: /app/data]
```

---

## 🌐 Docker Networking

### 🔹 Bridge Network (default)

```bash
docker network ls
docker network inspect bridge
docker run --network bridge nginx
```

### 🔹 Custom User-Defined Bridge Network

```bash
docker network create mynet
docker run -d --network mynet --name nginx1 nginx
```

### 🔹 Host Network (Linux only)

```bash
docker run --network host nginx
```

### 🔹 None Network (No network)

```bash
docker run --network none nginx
```

### 🔹 Connect Containers

```bash
docker network connect mynet mynodeapp
```

### 📊 Diagram: Docker Networking

```
[Bridge Network]
   ├── [Container A: nginx]
   └── [Container B: nodeapp]
      └── Shared via custom bridge network
```

---

## 🧩 Docker Compose

### 🔹 `docker-compose.yml`

```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
    networks:
      - mynet
  app:
    build: .
    volumes:
      - ./data:/app/data
    networks:
      - mynet
networks:
  mynet:
volumes:
  datavol:
```

### 🔹 Commands

```bash
docker-compose up -d
docker-compose ps
docker-compose down
```

### 📊 Diagram: Docker Compose

```
[docker-compose.yml]
    ├── Service: web (nginx)
    ├── Service: app (custom build)
        └── Volume: ./data <-> /app/data
        └── Network: mynet (shared)
```

---

## ☸️ Kubernetes Coordination

### 🔹 Deployment YAML Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: mynodeapp:latest
        ports:
        - containerPort: 3000
        volumeMounts:
        - name: data-vol
          mountPath: /app/data
      volumes:
      - name: data-vol
        persistentVolumeClaim:
          claimName: pvc-data
```

### 🔹 PersistentVolumeClaim Example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-data
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### 🔹 Service YAML (Expose App)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 32000
```

### 📊 Diagram: Docker to Kubernetes

```
[Docker Container]
    ├── Image: mynodeapp
    └── Volume: /app/data
       │
       ▼
[Kubernetes Pod]
    ├── Deployment: 2 replicas
    ├── PVC: pvc-data (1Gi)
    └── Service: NodePort (32000)
```

---
