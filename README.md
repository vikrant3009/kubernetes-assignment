# Kubernete 101 - Minikube setup, Application Deployment and Ingress Setup.
## Kubernetes Cluster Setup - Installation of Minikube, Kubectl and Helm

This guide explains how to set up a Kubernetes cluster using Minikube, how to install `kubectl` to interact with the cluster, and how to install Helm for managing Kubernetes applications.

### Prerequisites

- Ubuntu system with sudo privileges
- Internet connection

### Step 1: Kubernetes Cluster Setup - Installation of Minikube

#### 1. Update Packages

```bash
sudo apt update
```

This command updates the package lists on your system to ensure you have the latest information on the newest versions of packages and their dependencies.

#### 2. Login to Root

```bash
sudo su -
```

This command switches your current user to the root user, allowing you to perform administrative tasks without needing to use `sudo` for each command.

#### 3. Install Docker and Docker.io

```bash
apt install docker docker.io
```

This command installs Docker, a container runtime, which is required for running Minikube. `docker` and `docker.io` are package names for Docker on Ubuntu.

#### 4. Install curl and wget Utilities

```bash
apt install curl wget
```

This command installs `curl` and `wget`, which are utilities for downloading files from the internet.

#### 5. Download and Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```
These commands download the latest Minikube Debian package and install it on your system. Minikube is a tool that makes it easy to run a local Kubernetes cluster.

#### 6. Start Minikube Cluster

```bash
minikube start --driver=docker --force
```

This command starts a Minikube cluster using Docker as the driver. The `--force` option is used to bypass any pre-checks and force the cluster to start.

#### 7. Verify Minikube Setup

```bash
minikube status
```
  ![image](https://github.com/user-attachments/assets/f2715c3a-cc3e-48c2-805f-123b071502aa)

### Step 2: Install kubectl to Interact with the Cluster

#### 1. Install kubectl using snap utility

```bash
snap install kubectl --classic
```

This command installs `kubectl` using Snap with the `--classic` option, which gives it full access to system resources. `kubectl` is the command-line tool for interacting with Kubernetes clusters.

#### 2. Verify the kubectl Installation

```bash
kubectl cluster-info
```
![image](https://github.com/user-attachments/assets/f9403856-4e18-403a-b955-59c3a85ca9a8)


These commands verify that `kubectl` is installed correctly and can communicate with the Kubernetes cluster. `kubectl cluster-info` displays information about the cluster, and `kubectl get pods -A` lists all the pods in all namespaces.

### Step 3: Install Helm

#### 1. Install Helm using snap

```bash
snap install helm --classic
```
This command installs Helm using Snap with the `--classic` option. Helm is a package manager for Kubernetes, which helps in deploying and managing applications on the cluster.

#### 2. Verify Helm installation
```bash
helm version
```
![image](https://github.com/user-attachments/assets/59282092-094e-437e-8e26-bfd959a124f7)

### Conclusion

By following these steps, you can set up a local Kubernetes cluster using Minikube, install `kubectl` to interact with the cluster, and install Helm for managing applications. This setup provides a robust environment for learning and experimenting with Kubernetes.

