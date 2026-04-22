# Jenkins Pipeline with Docker Integration

## Overview

This exercise demonstrates a complete CI/CD pipeline that integrates GitHub, Jenkins, and Docker. The pipeline automatically pulls code from GitHub, builds a Docker image, and runs the containerized application.

### Workflow

```
GitHub Repository → Jenkins Pipeline → Docker Build → Container Execution
```

## Prerequisites

- Docker Desktop installed and running
- GitHub account with a valid repository
- Internet connectivity
- PowerShell or Bash terminal access

## Setup Instructions

### Step 1: Start Jenkins Container

Create a Docker volume for persistent Jenkins data:

```bash
docker volume create jenkins-data
```

Start the Jenkins container:

```bash
docker run -d `
  --name jenkins `
  -p 8080:8080 -p 50000:50000 `
  -v jenkins-data:/var/jenkins_home `
  -v /var/run/docker.sock:/var/run/docker.sock `
  jenkins/jenkins:lts
```

### Step 2: Retrieve Initial Password

Retrieve the initial admin password:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

1. Open your browser and navigate to `http://localhost:8080`
2. Paste the retrieved password
3. Install suggested plugins
4. Create a new user account

### Step 3: Configure Docker Access in Jenkins

Install Docker inside the Jenkins container:

```bash
docker exec -u root jenkins bash -c "apt-get update && apt-get install -y docker.io"
```

Grant proper permissions:

```bash
docker exec -u root jenkins chmod 666 /var/run/docker.sock
```

**Note:** This step is critical. Without proper Docker permissions, the pipeline will fail.

### Step 4: Prepare GitHub Repository

Create a new GitHub repository and add the following files:

#### `app.py`
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Jenkins + Docker!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

#### `requirements.txt`
```
flask
```

#### `Dockerfile`
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

#### `Jenkinsfile`
```groovy
pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/<repo-name>.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 5000:5000 myapp'
            }
        }
    }
}
```

**Important:** Replace `<your-username>` and `<repo-name>` with your GitHub credentials.

### Step 5: Create Jenkins Pipeline

1. Go to the Jenkins Dashboard
2. Click **New Item**
3. Enter pipeline name: `docker-github-pipeline`
4. Select **Pipeline**
5. Click **OK**

### Step 6: Configure Pipeline Source

1. Select **Pipeline script from SCM**
2. Choose **Git** as the SCM
3. Enter your GitHub repository URL
4. Set branch to `*/main`
5. Set script path to `Jenkinsfile`
6. Click **Save**

### Step 7: Execute Pipeline

1. Click **Build Now** to start the pipeline execution
2. Monitor the console output for progress

### Step 8: Verify Execution

Verify the Docker container is running:

```bash
docker ps
```

Open your browser and navigate to `http://localhost:5000` to confirm the application is running.

Expected output: `Hello from Jenkins + Docker!`

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| **Empty repository error** | Ensure your GitHub repo contains at least a README and Jenkinsfile |
| **Wrong branch name (main vs master)** | Verify your default branch name is `main` in GitHub settings |
| **Jenkinsfile not found** | Confirm the Jenkinsfile is named exactly `Jenkinsfile` (case-sensitive) at the repository root |
| **Docker permission denied** | Run: `docker exec -u root jenkins chmod 666 /var/run/docker.sock` |
| **Pipeline fails to clone repo** | Verify the GitHub repository URL is correct and accessible |

## Cleanup

To stop and remove the application container:

```bash
docker ps                          # Find container ID
docker stop <container-id>         # Stop container
docker rm <container-id>           # Remove container
```

To remove the Jenkins container:

```bash
docker stop jenkins
docker rm jenkins
docker volume rm jenkins-data
```

## Success Criteria

After completing this exercise, you should have:

✓ Integrated GitHub repository with Jenkins  
✓ Automatically built Docker images from source code  
✓ Successfully deployed containerized applications  
✓ Verified application functionality via web interface
