# 🧪 Experiment 2: Containerising a Flask Application using Docker

## 🎯 Aim

To containerise a Flask application using Docker and deploy the image to Docker Hub.

---

## 🛠️ Requirements

* Docker installed
* Docker Hub account

---

## 📄 Step 1: Create Flask Application

### app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Dockerised Flask!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

## 📄 Step 2: Create requirements.txt

```text
flask
```

---

## 🐳 Step 3: Create Dockerfile

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

## 🏗️ Step 4: Build Docker Image

```bash
docker build -t YOUR-DOCKER-USERNAME/flask-app:latest .
```

---

## ▶️ Step 5: Run Container Locally

```bash
docker run -d -p 5000:5000 YOUR-DOCKER-USERNAME/flask-app:latest
```

👉 Open browser:
http://localhost:5000

---

## 🔐 Step 6: Login to Docker Hub

```bash
docker login
```

---

## 🚀 Step 7: Push Image to Docker Hub

```bash
docker push YOUR-DOCKER-USERNAME/flask-app:latest
```

---

## ✅ Expected Output

* Flask app runs successfully in browser
* Output displayed: **Hello from Dockerised Flask!**
* Docker image successfully pushed to Docker Hub

---

## ⚠️ Common Errors

* ❌ Dockerfile named incorrectly (must be `Dockerfile`)
* ❌ Forgot `.` in build command
* ❌ Port not mapped correctly (`-p 5000:5000`)
* ❌ Not logged in before push

---

## 🧠 Viva Questions

**1. What is Docker?**
A tool used to create and run containers.

**2. What is a container?**
A lightweight environment to run applications.

**3. What is Dockerfile?**
A file containing instructions to build a Docker image.

**4. What is an image?**
A blueprint used to create containers.

**5. What is Docker Hub?**
A cloud platform to store and share Docker images.
