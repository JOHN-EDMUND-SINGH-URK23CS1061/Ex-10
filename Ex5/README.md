# 📄 Experiment Q06 – Kubernetes using kubectl

---

## 🎯 Objective

To use kubectl commands to:
- Create a deployment
- Scale pods
- Expose services
- Inspect Kubernetes resources
- Manage deployments using YAML

---

## 🧠 Concept

**Kubernetes** is used to manage containerized applications.

**kubectl** is the command-line tool used to interact with the Kubernetes cluster.

---

## 🧰 Prerequisites

Make sure the following are installed and configured:
- Docker Desktop is installed
- Kubernetes is enabled (in Docker Desktop)
- System has at least 4GB RAM
- kubectl is installed

---

## 🚨 IMPORTANT (VERY COMMON ERROR)

If you see this error:

```
Unable to connect to the server: EOF
```

**👉 This means Kubernetes is NOT running.**

---

## 🟢 STEP 1 – Start Kubernetes (Skip if already running)

1. Open Docker Desktop  
2. Go to **Settings → Kubernetes**  
3. Enable the following:
   - ✔ Enable Kubernetes  
   - ✔ Select **Kubeadm**
4. Click **Apply & Restart**

⏳ **Wait 3–5 minutes** until it shows **Running**

---

## 🟢 STEP 2 – Verify Kubernetes

Open a terminal and run:

```bash
kubectl get nodes
```

**Expected Output:** Shows nodes with status `Ready`

---

## 📍 STEP 3 – Go to Working Folder

Open terminal in your working folder:

```bash
cd "C:\Users\edmun\OneDrive\Desktop\devops dummy"
```

**👉 All files must be created here**

---

## 🚀 STEP 4 – Create Deployment

Run the following command to create a deployment:

```bash
kubectl create deployment myapp --image=nginx:latest
```

**Check the deployment and pods:**

```bash
kubectl get deployments
kubectl get pods
```

---

## 🔼 STEP 5 – Scale Up

Scale the deployment to 3 replicas:

```bash
kubectl scale deployment myapp --replicas=3
kubectl get pods
```

---

## 🔽 STEP 6 – Scale Down

Scale the deployment back to 1 replica:

```bash
kubectl scale deployment myapp --replicas=1
kubectl get pods
```

---

## 🌐 STEP 7 – Expose Service

Expose the deployment as a service:

```bash
kubectl expose deployment myapp --port=80 --type=NodePort
kubectl get services
```

---

## 🌍 STEP 8 – Access Application

Get the service details:

```bash
kubectl get svc myapp
```

**👉 Note the NodePort** (example: 30045)

Open your browser and navigate to:

```
http://localhost:30045
```

**👉 The nginx page should appear**

---

## 🔍 STEP 9 – Inspect Resources

Get the pod name:

```bash
kubectl get pods
```

Describe the pod:

```bash
kubectl describe pod <pod-name>
```

View logs:

```bash
kubectl logs <pod-name>
```

---

## 📄 STEP 10 – YAML METHOD

### Create a deployment.yaml file

Open a text editor in your working folder:

```bash
notepad deployment.yaml
```

**Paste the following YAML content:**

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
        image: nginx:latest
        ports:
        - containerPort: 80
```

Save the file.

### Apply the YAML

Run the following commands:

```bash
kubectl apply -f deployment.yaml
kubectl get all
```

---

## 🧹 STEP 11 – Clean Up

Delete the deployment and service:

```bash
kubectl delete deployment myapp
kubectl delete service myapp
```

---

## ⚠️ COMMON ERRORS & FIXES

### ❌ Error: EOF / Cannot connect

**✔ Fix:**
- Start Kubernetes in Docker Desktop
- Wait properly (3–5 minutes)

### ❌ Kubernetes not starting

**✔ Fix:**
- Restart Docker Desktop
- Reset Kubernetes cluster
- Ensure system has enough RAM

### ❌ YAML file not found

**✔ Fix:**
- Create the file in the SAME folder where the terminal is running

### ❌ Port not opening

**✔ Fix:**
- Check the NodePort using:

```bash
kubectl get svc
```

### ❌ Pods not running

**✔ Fix:**
- Use this command to inspect the pod:

```bash
kubectl describe pod <pod-name>
```

---

## 🧠 Expected Output

- ✅ Deployment created
- ✅ Pods running
- ✅ Scaling working
- ✅ Service exposed
- ✅ nginx page visible

---

## 🏁 Result

Successfully used kubectl commands to create, scale, expose, and manage Kubernetes resources.