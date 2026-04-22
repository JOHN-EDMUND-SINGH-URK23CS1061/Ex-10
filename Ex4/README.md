# 🧪 Experiment 4: CI Pipeline using GitHub Actions + Docker

## 🎯 Aim

To create a CI pipeline using GitHub Actions to build and push a Docker image of a Python (Flask) app to Docker Hub.

---

## 📖 Theory

CI (Continuous Integration) automatically builds and deploys code whenever changes are pushed to the repository.
Here, GitHub Actions is used to build a Docker image and push it to Docker Hub.

---

# 🛠️ Step 1: Create Project Files

## 📄 app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "CI Pipeline Working!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

## 📄 requirements.txt

```text
flask
```

---

## 🐳 Dockerfile

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

# 🧱 Step 2: Push Project to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M master
git remote add origin https://github.com/YOUR-USERNAME/Devops-Lab.git
git push -u origin master
```

---

# 🔐 Step 3: Create Docker Hub Access Token

Go to Docker Hub:

1. Account Settings → Security
2. Personal Access Tokens
3. Create new token
4. Select **Read & Write**
5. Copy token (IMPORTANT)

---

# 🔑 Step 4: Add Secrets in GitHub

Go to GitHub:

Settings → Secrets → Actions → New repository secret

Add:

```bash
DOCKER_USERNAME = your_docker_username
DOCKER_PASSWORD = your_access_token
```

---

# ⚙️ Step 5: Create GitHub Actions Workflow

Create file:

```
.github/workflows/docker-publish.yml
```

```yaml
name: Docker CI

on:
  push:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - run: docker build -t ${{ secrets.DOCKER_USERNAME }}/python-app .
      - run: docker push ${{ secrets.DOCKER_USERNAME }}/python-app
```

---

# 🚀 Step 6: Trigger CI Pipeline

## Method 1 (Easiest)

* Create or edit `README.md` in GitHub
* Click **Commit changes**

## Method 2 (Terminal)

```bash
git add .
git commit -m "trigger CI"
git push
```

---

# 📊 Step 7: Verify Execution

1. Go to **Actions tab** in GitHub
2. Check workflow status (🟢 Success)
3. Go to Docker Hub → verify image exists

---

# ✅ Expected Output

* CI pipeline runs automatically on push
* Docker image is built successfully
* Image is pushed to Docker Hub

---

# ⚠️ Common Errors & Fixes (VERY IMPORTANT)

## ❌ Error: Repository not found

👉 Cause:
Used `YOUR-USERNAME` instead of actual username

👉 Fix:

```bash
git remote remove origin
git remote add origin https://github.com/<your-username>/Devops-Lab.git
```

---

## ❌ Error: Docker not running

```bash
failed to connect to docker API
```

👉 Fix:

* Open Docker Desktop
* Wait until it starts

---

## ❌ Error: curl not working (PowerShell issue)

👉 Cause:
PowerShell uses `Invoke-WebRequest` instead of curl

👉 Fix:
Use PowerShell commands or use CMD

---

## ❌ Error: Permission denied (Docker push)

👉 Cause:

* Used password instead of token
* Token had Read-only access

👉 Fix:

* Use **Access Token**
* Set permission to **Read & Write**

---

## ❌ Error: CI not running

👉 Cause:
No push happened

👉 Fix:

* Edit any file and commit

---

## ❌ Error: Workflow not detected

👉 Cause:
Wrong file path

👉 Fix:
File must be:

```
.github/workflows/docker-publish.yml
```

---

# 🔐 Security Note

* Never share Docker token publicly
* If exposed → revoke immediately and create new

---

# 🧠 Viva Questions

**1. What is CI?**
Continuous Integration — automatic build/testing on code changes.

**2. What is GitHub Actions?**
Tool to automate workflows in GitHub.

**3. What is Docker?**
Platform to containerize applications.

**4. Why use secrets?**
To securely store credentials.

**5. What triggers the pipeline?**
Push event in repository.

---

# 💯 Conclusion

Successfully created a CI pipeline that automatically builds and pushes a Docker image to Docker Hub on every push to the repository.
