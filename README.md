
# Kuberentes Assignment

This is the assignment given by Ashnik Technologies to for the role of Kubernetes & Nginx Specialist.


## 1. Kubernetes Cluster Setup: Installation of minikube

Update packages
```bash
  sudo apt update
```
Login to root
```bash
  sudo su -
```
Install docker and docker.io
```bash
  apt install docker docker.io
```
Install curtl and wget utilities
```bash
  apt install curl wget
```
Download and install Minikube
```bash
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
  sudo dpkg -i minikube_latest_amd64.deb
```
Start minikube cluster
```bash
  minikube start --driver=docker --force
```
Install kubectl to interact with cluster
```bash
  snap install kubectl --classic
```
Verify the installation
```bash
  kubectl cluster-info
  kubectl get pods -A
```

