# Nginx Ingress Controller Deployment with Helm

This guide explains how to set up Nginx Ingress Controller on a Kubernetes cluster using Helm. Ingress Nginx is an Ingress controller that manages access to your Kubernetes services from outside the Kubernetes cluster.

## Prerequisites

- Kubernetes/Minikube cluster
- Helm installed and configured

## Commands Explanation

### 1. Add Ingress Nginx Helm Repository

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

This command adds the Ingress Nginx Helm repository to your local Helm client. The repository contains the necessary charts to deploy Ingress Nginx.

### 2. Update Helm Repositories

```bash
helm repo update
```

This command updates your local Helm repository cache with the latest charts available in the remote repositories. It ensures that you have access to the most recent versions of the charts.

### 3. Install or Upgrade Ingress Nginx

```bash
helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace \
  --set controller.kind=DaemonSet \
  --set controller.hostPort.enabled=true \
  --set controller.ingressClass=nginx
```

This command installs or upgrades the Ingress Nginx controller using Helm. Here’s a breakdown of the options used:

- `upgrade --install`: This command will upgrade the release if it already exists or install it if it does not.
- `ingress-nginx`: This is the name of the release.
- `ingress-nginx/ingress-nginx`: Specifies the chart to install. The first part is the repository name, and the second part is the chart name.
- `--namespace ingress-nginx`: Specifies the namespace in which to install the Ingress Nginx controller. If the namespace does not exist, it will be created.
- `--create-namespace`: This option creates the namespace if it does not already exist.
- `--set controller.kind=DaemonSet`: This sets the Ingress Nginx controller to run as a DaemonSet, ensuring that a pod is running on each node.
- `--set controller.hostPort.enabled=true`: This enables the use of host ports for the Ingress Nginx controller, allowing it to bind to the host’s network interface.
- `--set controller.ingressClass=nginx`: This sets the ingress class to `nginx`, which is used to differentiate between multiple ingress controllers.

### 3. Verify the Nginx Ingress Controller Setup

```bash
kubectl get all -n ingress-nginx
```

![image](https://github.com/user-attachments/assets/5cfe1f18-fb53-4890-942d-852c502ddc9d)

## Conclusion

By following these steps, you can successfully deploy the Ingress Nginx controller on your Kubernetes cluster using Helm. This setup provides a reliable and efficient way to manage external access to your Kubernetes services.
