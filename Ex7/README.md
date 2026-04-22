# 📄 Experiment Q07 – GitHub Actions Pipeline (Docker Build & Push)

---

## 🎯 Objective

To create a GitHub Actions pipeline that:

- Automatically builds a Docker image
- Pushes the image to Docker Hub
- Triggers on every push to the main branch

---

## 🧠 Concept (Simple)

This experiment demonstrates **CI/CD using GitHub Actions**:

```
GitHub → GitHub Actions → Docker Build → Push to Docker Hub
```

---

## 🧰 Prerequisites

Make sure you have:

- GitHub account
- Docker Hub account
- Basic internet connection

---

## 🚀 Step 1 – Create Docker Hub Account

**Action:**
1. Go to: https://hub.docker.com
2. Sign up / Login
3. **Note your username** (you'll need it later)

---

## 🔐 Step 2 – Generate Docker Access Token (IMPORTANT)

⚠️ **DO NOT use your password!**

**Action:**
1. Go to Docker Hub → Profile → Account Settings → Security  
2. Click **New Access Token**  
3. Set Name: `github-actions`  
4. Set Permission: **Read & Write**  
5. Click **Generate**  
6. **Copy the token** and save it somewhere safe

---

## 📁 Step 3 – Prepare GitHub Repository

**Your repo MUST contain these three files:**

### 📄 File: `app.py`

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from GitHub Actions!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

### 📄 File: `requirements.txt`

```
flask
```

### 📄 File: `Dockerfile`

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install flask
CMD ["python", "app.py"]
```
**⚠️ Important Requirements:**

- ✅ Repo must **NOT** be empty
- ✅ Branch must be **main**
- ✅ All three files must exist (app.py, requirements.txt, Dockerfile)

---

## ⚙️ Step 4 – Create GitHub Actions Workflow

**Action:**
1. Create folder: `.github/workflows/`
2. Create file: `.github/workflows/build-push.yml`
3. Copy the following code:

### 📄 File: `.github/workflows/build-push.yml`

```yaml
name: Docker Build & Push

on:
  push:
    branches: [ main ]

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and Push Image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/python-app:latest
```
---

## 🔐 Step 5 – Add GitHub Secrets (VERY IMPORTANT)

**Action:**
1. Go to: **GitHub Repo → Settings → Secrets and Variables → Actions**
2. Add the following two secrets:

### Secret 1: `DOCKER_USERNAME`

| Property | Value |
|----------|-------|
| **Name** | `DOCKER_USERNAME` |
| **Value** | Your Docker Hub username |

### Secret 2: `DOCKER_PASSWORD`

| Property | Value |
|----------|-------|
| **Name** | `DOCKER_PASSWORD` |
| **Value** | Paste the Access Token you created in Step 2 |

⚠️ **Remember:** Use the **Access Token**, NOT your Docker password!

---

## 🚀 Step 6 – Trigger the Pipeline

**Action:**
Make any change and push to the main branch:

```bash
git add .
git commit -m "trigger pipeline"
git push
```

---

## 🔍 Step 7 – Check Workflow Status

**Action:**
1. Go to: **GitHub → Actions tab**
2. You should see the workflow running
3. Check that **all steps are successful** ✅

---

## 📦 Step 8 – Verify Image in Docker Hub

**Action:**
1. Go to: https://hub.docker.com
2. Look for: `python-app` in your repositories
3. Confirm the image exists with the tag `:latest`

---

## ⚠️ Common Errors (IMPORTANT FOR EXAM)

### ❌ Error: Using Docker Password Instead of Access Token

**Problem:** Authentication fails when pushing to Docker Hub

**Solution:**
- ✅ Use **Access Token** only (generated in Step 2)
- ❌ Do NOT use your Docker Hub password

### ❌ Error: Wrong Secret Names

**Problem:** Workflow can't find the secrets

**Solution:**
- ✅ Secret names must be **EXACTLY**:
  - `DOCKER_USERNAME`
  - `DOCKER_PASSWORD`

### ❌ Error: Repository is Empty

**Problem:** Workflow won't trigger on push

**Solution:**
- ✅ Add the three required files before running
  - app.py
  - requirements.txt
  - Dockerfile

### ❌ Error: Using Wrong Branch

**Problem:** Workflow doesn't trigger

**Solution:**
- ✅ Make sure you're pushing to the **main** branch
- ✅ Check `.github/workflows/build-push.yml` has correct branch configuration

### ❌ Error: Workflow File in Wrong Location

**Problem:** GitHub Actions doesn't find the workflow

**Solution:**
- ✅ File **MUST** be at: `.github/workflows/build-push.yml`
- ✅ Check the exact path and spelling

---

## 🧠 Expected Output

When everything is set up correctly, you should see:

✅ Code checkout  
✅ Docker login successful  
✅ Docker image built successfully  
✅ Image pushed to Docker Hub  

---

## 🏁 Final Result

You have successfully created a **CI/CD pipeline using GitHub Actions** that:

- Automatically detects pushes to the main branch
- Builds a Docker image from your code
- Authenticates with Docker Hub
- Pushes the image to your Docker Hub registry
- Makes the image available for deployment

**Congratulations!** Your automated Docker build pipeline is now live. 🎉