# Jenkins-Docker

This repository provides a complete step-by-step guide for installing and running **Jenkins locally using Docker**, along with a custom Dockerfile that includes Docker inside the Jenkins container (Docker-in-Docker setup).

---

## 🚀 Overview

Using this setup, you can:

* Build a Jenkins server in Docker
* Run Jenkins jobs that require Docker commands
* Persist Jenkins data locally
* Access Jenkins through your browser

---

## 📦 Step 1: Build the Dockerfile

Your custom Dockerfile installs Docker inside the Jenkins container.

To build the Docker image:

```bash
docker build -t jenkins [PATH-TO-DOCKERFILE]
```

This creates a Jenkins image named **jenkins** with Docker preinstalled.

---

## ▶️ Step 2: Start the Jenkins Container

Run the Jenkins container using:

```bash
docker run -p 8080:8080 -p 50000:50000 \
  -v "/c/Users/Theodoros/Desktop/Install Jenkins/Jenkins-Docker:/var/jenkins_home" \
  jenkins/jenkins
```

### What this command does:

* `-p 8080:8080` → Maps the Jenkins web UI to `localhost:8080`
* `-p 50000:50000` → Required for Jenkins agent communication
* `-v <host-path>:/var/jenkins_home` → Persists Jenkins data on your machine

---

## 🌐 Step 3: Access Jenkins

Once the container is running, open:

```
http://localhost:8080
```

You will be asked for the Jenkins initial admin password.
To find it, run:

```bash
docker logs [container_id]
```

Look for a log line similar to:

```
Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation: [password]
```

Copy that password and paste it into the browser prompt.

---

## 🔌 Step 4: Install Plugins

Jenkins supports a powerful plugin system.

To install plugins:

1. Go to **Manage Jenkins**
2. Select **Manage Plugins**
3. Browse, install, and update plugins as needed

You can now fully configure Jenkins for CI/CD workflows.

---

## 🧱 Included Dockerfile

This is the Dockerfile from your repo:

```Dockerfile
# Use the official Jenkins image as the base
FROM jenkins/jenkins:lts

USER root

# Install Docker
RUN apt-get update && apt-get install -y docker.io

# Start the Docker daemon
RUN dockerd &

# Expose the Docker socket
VOLUME /var/run/docker.sock:/var/run/docker.sock
```

This enables the container to run Docker commands required for CI/CD pipelines.

---

## 🎯 Summary

With this setup you have:

* A Jenkins server running in Docker
* Docker installed inside Jenkins for pipeline builds
* All Jenkins data stored locally
* A quick and reusable development pipeline environment
