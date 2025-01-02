# Kubernetes Architecture: A Docker User's Upgrade Guide

Kubernetes (K8s) is a powerful open-source platform designed to automate deploying, scaling, and operating application containers. For those with a decent understanding of Docker, learning Kubernetes can significantly enhance your ability to manage containerized applications at scale.

This guide provides an overview of Kubernetes architecture and includes practical examples to help you transition from Docker to Kubernetes.

---

## Core Components of Kubernetes Architecture

### 1. **Master Node**
The master node orchestrates the cluster, managing the worker nodes and their workloads.

#### Key Components:
- **API Server**: The gateway for all administrative tasks. It processes REST requests and communicates with the cluster’s components.
- **Scheduler**: Assigns workloads (Pods) to nodes based on resource availability.
- **Controller Manager**: Ensures the cluster's desired state by handling tasks like node health monitoring and replication.
- **etcd**: A distributed key-value store that holds the cluster's configuration and state.

#### Example:
If you define a deployment in Kubernetes, the API server processes the request, the scheduler assigns it to a node, and the controller manager ensures the required number of replicas are running.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.17
        ports:
        - containerPort: 80
```

### 2. **Worker Nodes**
Worker nodes execute the containerized workloads.

#### Key Components:
- **Kubelet**: Communicates with the API server to ensure the containers are running as specified.
- **Kube-Proxy**: Manages networking and facilitates communication between Pods and Services.
- **Container Runtime**: Runs the containers (e.g., Docker, containerd).

#### Example:
When a Pod is scheduled to a worker node, the Kubelet pulls the container images and starts the containers.

---

## Kubernetes Objects

### 1. **Pods**
A Pod is the smallest deployable unit in Kubernetes. It encapsulates one or more containers.

#### Example:
A simple Pod definition to run an Nginx container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.17
    ports:
    - containerPort: 80
```

### 2. **Services**
Services provide stable network endpoints to Pods.

#### Example:
Expose the `nginx-pod` with a Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP
```

### 3. **Deployments**
Deployments manage ReplicaSets, ensuring a desired number of Pods.

#### Example:
Deploy Nginx with 3 replicas:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.17
        ports:
        - containerPort: 80
```

### 4. **ConfigMaps and Secrets**
ConfigMaps store non-sensitive configuration data, while Secrets store sensitive data (e.g., passwords).

#### Example:
Create a ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: default
  data:
    database_url: "postgres://user@host:5432/dbname"
```

---

## Transitioning from Docker to Kubernetes

### Running Containers
In Docker:
```bash
docker run -d --name nginx -p 80:80 nginx:1.17
```

In Kubernetes (using a Pod):
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.17
    ports:
    - containerPort: 80
```
Apply with:
```bash
kubectl apply -f nginx-pod.yaml
```

### Scaling
In Docker Compose:
```yaml
services:
  web:
    image: nginx:1.17
    deploy:
      replicas: 3
```

In Kubernetes (Deployment):
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.17
```

---

## Conclusion
Kubernetes builds on Docker's containerization capabilities, adding robust orchestration, scaling, and management features. By understanding the core components and practicing with real-world examples, you can upgrade your container management skills and harness the full potential of Kubernetes.

---

Happy learning!
