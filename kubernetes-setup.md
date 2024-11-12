
# Setting Up Kubernetes on Linux-Ubuntu

This guide provides step-by-step instructions for setting up Kubernetes on Linux including all necessary dependencies.

## Table of Contents


- [Linux Setup](#linux-setup)
  - [Step 1: Install Dependencies](#step-1-install-dependencies-1)
  - [Step 2: Install kubectl](#step-2-install-kubectl-1)
  - [Step 3: Install Minikube (Optional)](#step-3-install-minikube-optional-1)
  - [Step 4: Start Minikube](#step-4-start-minikube-1)
  - [Step 5: Verify Installation](#step-5-verify-installation-1)


---

## Linux Setup

### Step 1: Install Dependencies

1. **Install Docker**:
   - For Ubuntu, run:

     ```bash
     sudo apt-get update
     sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
     sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
     sudo apt-get update
     sudo apt-get install -y docker-ce
     ```

   - Add your user to the Docker group:

     ```bash
     sudo usermod -aG docker $USER
     ```

2. **Install VirtualBox** (if using Minikube with VirtualBox):
   - For Ubuntu, run:

     ```bash
     sudo apt-get install -y virtualbox virtualbox-ext-pack
     ```

### Step 2: Install kubectl

1. **Download kubectl**:

   ```bash
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   ```

2. **Install kubectl**:

   ```bash
   chmod +x kubectl
   sudo mv kubectl /usr/local/bin/
   ```

### Step 3: Install Minikube (Optional)

1. **Download Minikube**:

   ```bash
   curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   ```

2. **Install Minikube**:

   ```bash
   chmod +x minikube
   sudo mv minikube /usr/local/bin/
   ```

### Step 4: Start Minikube

1. **Start Minikube**:

   ```bash
   minikube start --driver=virtualbox
   ```

   - Replace `--driver=virtualbox` with `--driver=docker` if using Docker as the driver.

### Step 5: Verify Installation

1. **Verify kubectl**:

   ```bash
   kubectl version --client
   ```

2. **Verify Minikube**:

   ```bash
   minikube status
   ```

---
