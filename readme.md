
# 🛠️ Jenkins + Docker + Flask CI/CD Setup on Ubuntu EC2

This guide walks you through setting up Jenkins and Docker on an Ubuntu EC2 instance, and configuring a Jenkins job to build and run a Flask app with Pytest and Docker.

---

## 🚀 1. Launch EC2 Instance

- **AMI**: Ubuntu 20.04 or 22.04
- **Instance Type**: t2.micro (for testing)
- **Key Pair**: Choose or create one
- **Security Group (Add Inbound Rules)**:
  | Type        | Protocol | Port Range  | Source |
  |-------------|----------|-------------|--------|
  | SSH         | TCP      | 22          | 0.0.0.0/0 (or your IP) |
  | HTTP        | TCP      | 80          | 0.0.0.0/0 |
  | HTTPS       | TCP      | 443         | 0.0.0.0/0 |
  | Custom TCP  | TCP      | 8080        | 0.0.0.0/0 |

---

## 🧰 2. Install Dependencies

### 🔹 Update Packages
https://www.jenkins.io/doc/book/installing/linux/#installation-of-java

## 📦 3. Install Jenkins
https://www.jenkins.io/doc/book/installing/linux/#debian-stable


## 🐍 4. Install Python, Pip, Pytest

```bash
sudo apt install python3-pip -y
pip3 --version

sudo apt install python3-pytest -y
pytest --version

sudo apt install python3-flask -y
flask --version
```

---

## 🐳 5. Install Docker (Official Method)

### 🔹 Follow Docker Docs:

(https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)


---

## 👤 6. Give Jenkins Access to Docker

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

## 🌐 7. Access Jenkins

* Go to: `http://<EC2-Public-IP>:8080`

### 🔹 Get Initial Admin Password:

## 🛠️ 8. Configure Jenkins Job

### 🔸 Create a Job

1. Click **New Item**
2. Enter name: `flask-cicd`
3. Select **Freestyle project** → OK

### 🔸 Configure Job

#### 🔹 Source Code Management:

* Choose: **Git**
* Repo URL:
  `https://github.com/piyushbankhede/Flask-CICD.git`
* Branches to build:
  `*/main`

#### 🔹 Build Steps:

Choose **"Execute shell"** and add the following:

```bash
echo Running tests...
pytest

echo Building Docker image...
docker build -t flaskappcicd .

echo Running container...
docker run -itd --name flaskc1 -p 5000:5000 flaskappcicd

echo "Container is created and running..."
```

---

## ✅ 9. Save and Build

* Click **Save**
* Click **Build Now**
* Go to **Build Console Output** to check logs

---

## 🌐 10. Access Your Flask App

Open in browser:

```
http://<EC2-Public-IP>:5000
```

## 11. Add Trigger
* Scroll to Build Triggers.
* Check "Poll SCM"

In the Schedule box, enter:
```
* * * * *
```

Poll every  minutes. 
---

## 🎉 Done!

You've now fully automated:

* Pulling code from GitHub
* Running tests with `pytest`
* Building Docker image
* Deploying Flask app in a container
* Triggered via Jenkins UI or webhook

---

## 🔁 Optional Next Steps

* Add GitHub Webhook for auto-trigger
* Push Docker image to DockerHub or AWS ECR
* Add Email or Slack notifications
* Add rollback step on test failure

---
