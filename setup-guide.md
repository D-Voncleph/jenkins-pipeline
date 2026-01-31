# Jenkins Server Setup & Automation Guide

**Date:** January 2026  
**Project:** FinTech DevOps Roadmap - Phase 3

## 1. Infrastructure Setup

I provisioned an EC2 instance (Ubuntu 22.04) using Terraform.

* **Region:** eu-north-1 (Stockholm)
* **Port:** Opened port `8080` in the Security Group.

## 2. Jenkins Installation (The "Clean" Method)

After encountering GPG signature errors with the default documentation, I used this robust method to install Jenkins on Ubuntu 22.04:

### Step A: Install Java

Jenkins requires Java to run.

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre -y
```

### Step B: Add the Jenkins Repository (Manually)

To avoid `NO_PUBKEY` errors, I manually downloaded the key and simplified the source list.

```bash
# 1. Remove conflicting files (if any)
sudo rm /etc/apt/sources.list.d/jenkins.list

# 2. Add the global key using the deprecated (but working) method for Ubuntu 22
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7198F4B714ABFC68

# 3. Add the repository source (Simple format)
echo "deb https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### Step C: Install Jenkins

```bash
sudo apt-get update
sudo apt-get install jenkins -y
```

### Step D: Unlock

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

## 3. Automation Configuration (CI)

### The Job: "FinTech-Pull-Test"

I created a Freestyle Project configured to:

1. **Source Code:** Pull from my `fintech-app-docker` GitHub repository.
2. **Branch:** `main`.

### The Trigger: GitHub Webhooks

To enable real-time builds (Push-to-Deploy):

1. **Jenkins:** Enabled "GitHub hook trigger for GITScm polling".
2. **GitHub:** Added a payload URL: `http://<SERVER_IP>:8080/github-webhook/`.

### Proof of Success

Below is the console output showing a successful `git clone` triggered automatically by a push event.

(Note: Rename your screenshot to match this, or update the path)

---
