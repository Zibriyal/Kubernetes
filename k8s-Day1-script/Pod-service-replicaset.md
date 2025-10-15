
---

## 🚀 Kubernetes Day 1 – Basic Pod, Service, and ReplicaSet Setup

This document captures the step-by-step process of creating a Pod, exposing it with a NodePort Service, scaling with a ReplicaSet, and understanding Kubernetes self-healing.

---

### 📁 Project Structure

```bash
E:\k8s-Day1-script
│
├── pod-example.yaml
├── service.yaml
├── nginx-replicaset.yaml
```

---

## 1️⃣ Create a Pod using YAML

### File: `pod-example.yaml`

```yaml
apiVersion: v1  
kind: Pod               
metadata:               
  name: my-pod          
  labels:               
    app: my-app         
spec: 
  containers:           
    - name: my-container 
      image: nginx:latest
```
![alt text](<Screenshot (240).png>)

### ✅ Apply Pod Configuration

```bash
cd E:\k8s-Day1-script
kubectl get all
kubectl apply -f pod-example.yaml
kubectl get all
```
![alt text](<Screenshot (241).png>)
![alt text](<Screenshot (242).png>)
![alt text](<Screenshot (243).png>)
![alt text](<Screenshot (244).png>)

---
* Check Container running
```bash
docker ps
```
![alt text](<Screenshot (245).png>)
---
* Take Info abot Pod

```bash
kubectl describe pod my-pod
```
![alt text](<Screenshot (246).png>)
---
---
## 2️⃣ Access the Pod's Container

```bash
kubectl exec -it my-pod -- /bin/bash
```

![alt text](<Screenshot (247).png>)

---

## 3️⃣ Expose Pod using NodePort Service

### File: `service.yaml`

screenshots/Screenshot (248).png

---

### ✅ Apply Service Configuration

```bash
kubectl apply -f service.yaml
kubectl get all
```

![alt text](<Screenshot (249).png>)
![alt text](<Screenshot (250).png>)


---

## 4️⃣ Test NGINX in Browser

Visit:

```
http://localhost:30007
```

![alt text](<Screenshot (251).png>)

---

## 5️⃣ Create ReplicaSet to Manage Pods

### File: `nginx-replicaset.yaml`

```yaml
apiVersion: apps/v1       
kind: ReplicaSet          
metadata:
  name: nginx-replicaset  
spec:
  replicas: 3             
  selector:
    matchLabels:          
      app: my-app
  template:               
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: nginx       
        image: nginx:latest 
        ports:
        - containerPort: 80
```

### ✅ Apply ReplicaSet Configuration

```bash
kubectl apply -f nginx-replicaset.yaml
kubectl get all
kubectl get replicaset
```

![alt text](<Screenshot (252).png>)
![alt text](<Screenshot (253).png>)
![alt text](<Screenshot (254).png>)
---
### ✅ Scaling Replicaset (Optional)

```bash
kubectl scale --replicas=5 replicaset/nginx-replicaset
kubectl get all
```
---

## 6️⃣ Test High Availability

### 🧪 Delete One Pod

```bash
kubectl delete pod nginx-replicaset-hzf8r
kubectl get all
```

✅ A new Pod will be auto-created to maintain 3 replicas.

![alt text](<Screenshot (255).png>)

---

## 7️⃣ Delete ReplicaSet and Cleanup

```bash
kubectl delete -f nginx-replicaset.yaml
kubectl get all
```

🧹 All 3 Pods are terminated. NGINX service should now show error in browser.

![alt text](<Screenshot (256).png>)
![alt text](<Screenshot (257).png>)
![alt text](<Screenshot (258).png>)

---

## 📌 Summary

* ✅ You created and deployed a basic Pod
* ✅ Exposed it with a NodePort service
* ✅ Scaled it using a ReplicaSet
* ✅ Tested Kubernetes' self-healing by deleting a Pod
* ✅ Cleaned up resources after use

---


## ✅ Conclusion: What I Learned

In this Day 1 hands-on Kubernetes session, I learned the basics of working with Kubernetes using real examples. I practiced creating Pods, exposing them using Services, and scaling with ReplicaSets. I also explored how Kubernetes ensures high availability and self-healing by automatically recreating deleted pods.

This gave me practical experience with the core building blocks of Kubernetes and helped me understand how container orchestration works in real-time.

---

## 🛠️ Tools Used

| Tool / Platform                              | Purpose                                               |
| -------------------------------------------- | ----------------------------------------------------- |
| **VS Code**                                  | Writing YAML files for Kubernetes                     |
| **kubectl**                                  | Kubernetes command-line tool to interact with cluster |
| **Docker Desktop** (with Kubernetes enabled) | Local Kubernetes environment                          |
| **Command Prompt / Terminal**                | Running Kubernetes commands                           |
| **Web Browser (Chrome)**                     | Testing the NodePort service with NGINX               |

---

## 💻 Kubernetes Commands Practiced

| Command                                    | Description                                             |
| ------------------------------------------ | ------------------------------------------------------- |
| `kubectl apply -f <file>`                  | Apply a configuration file (create or update resources) |
| `kubectl get all`                          | List all resources (pods, services, etc.)               |
| `kubectl get pods` / `replicaset`          | Check specific resource status                          |
| `kubectl describe pod <pod-name>`          | Show detailed info about a Pod                          |
| `kubectl exec -it <pod-name> -- /bin/bash` | Access the container’s shell                            |
| `kubectl delete pod <pod-name>`            | Delete a pod (ReplicaSet will recreate if managed)      |
| `kubectl delete -f <file>`                 | Delete resources defined in YAML file                   |

---
