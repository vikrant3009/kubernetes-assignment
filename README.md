# Kubernete 101 by Vikrant- Minikube setup, Ingress Controller Setup and Application Deployment.
## Part I: Kubernetes Cluster Setup - Installation of Minikube, Kubectl and Helm

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

-------

## Part II: Nginx Ingress Controller Deployment with Helm

This guide explains how to set up Nginx Ingress Controller on a Kubernetes cluster using Helm. Ingress Nginx is an Ingress controller that manages access to your Kubernetes services from outside the Kubernetes cluster.

### Prerequisites

- Kubernetes/Minikube cluster
- Helm installed and configured

### Commands Explanation

#### 1. Add Ingress Nginx Helm Repository

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

This command adds the Ingress Nginx Helm repository to your local Helm client. The repository contains the necessary charts to deploy Ingress Nginx.

#### 2. Update Helm Repositories

```bash
helm repo update
```

This command updates your local Helm repository cache with the latest charts available in the remote repositories. It ensures that you have access to the most recent versions of the charts.

#### 3. Install or Upgrade Ingress Nginx

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

#### 3. Verify the Nginx Ingress Controller Setup

```bash
kubectl get all -n ingress-nginx
```

![image](https://github.com/user-attachments/assets/5cfe1f18-fb53-4890-942d-852c502ddc9d)

### Conclusion

By following these steps, you can successfully deploy the Ingress Nginx controller on your Kubernetes cluster using Helm. This setup provides a reliable and efficient way to manage external access to your Kubernetes services.

-------------------------------------------------

## Part III: Hello World Application Deployment

This repository contains the YAML files required to deploy a simple "Hello World" application on a Kubernetes cluster. The deployment consists of a Deployment, a Service, and an Ingress resource.

### Deployment

The `deployment-hello-world.yaml` file defines a Kubernetes Deployment for the "Hello World" application.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world-deployment
  namespace: hello-world
  labels:
    app: hello-world
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: gcr.io/google-samples/hello-app:1.0
```

### Service

The `service-hello-world.yaml` file defines a Kubernetes Service to expose the "Hello World" application.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-svc
  namespace: hello-world
spec:
  selector:
    app: hello-world
  ports:
  - port: 80
    targetPort: 8080
```

### Ingress

The `ingress-hello-world.yaml` file defines a Kubernetes Ingress to route external traffic to the "Hello World" application.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  name: hello-world-ingress
  namespace: hello-world
spec:
  ingressClassName: nginx
  rules:
  - host: www.devopsbyvikrant.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hello-world-svc
            port:
              number: 80
```

### Usage

To deploy the "Hello World" application, follow these steps:

1. **Create the Namespace**:
    ```bash
    kubectl create namespace hello-world
    ```

2. **Apply the Deployment**:
    ```bash
    kubectl apply -f deployment-hello-world.yaml
    ```

3. **Apply the Service**:
    ```bash
    kubectl apply -f service-hello-world.yaml
    ```

4. **Apply the Ingress**:
    ```bash
    kubectl apply -f ingress-hello-world.yaml
    ```

### Verification

After applying the YAML files, you can verify the deployment by checking the status of the pods, service, and ingress:

1. **Check Pods**:
    ```bash
    kubectl get pods -n hello-world
    ```
   ![image](https://github.com/user-attachments/assets/0b4c1106-d25b-4acb-9fb8-e5f74b12513c)

2. **Check Service**:
    ```bash
    kubectl get svc -n hello-world
    ```
    ![image](https://github.com/user-attachments/assets/36028ed1-822a-49b7-a8e2-b98752af8e48)


3. **Check Ingress**:
    ```bash
    kubectl get ingress -n hello-world
    ```
    ![image](https://github.com/user-attachments/assets/b67e0e68-28be-4cf5-bba4-ffc139a78102)


Visit `http://www.devopsbyvikrant.com` in your browser to see the "Hello World" application.
![image](https://github.com/user-attachments/assets/da54cd20-62dd-408d-af4e-8f9a20e024bb)


### Cleanup

To remove the deployed resources, you can delete them using the following commands:

```bash
kubectl delete -f ingress-hello-world.yaml
kubectl delete -f service-hello-world.yaml
kubectl delete -f deployment-hello-world.yaml
kubectl delete namespace hello-world
```

### Troubleshooting

If you encounter any issues, try the following troubleshooting steps:

5. **DNS Issues**:
    If you are unable to access the application via the provided hostname, ensure that the DNS settings are correctly configured and that the hostname resolves to the correct IP address.
    Follow workaround in minikube host server:
    ```sh
    echo "$(minikube ip) www.devopsbyvikant.com" >> /etc/hosts
    curl -k http://www.devopsbyvikant.com
    ```
    ![image](https://github.com/user-attachments/assets/96f5b692-c40d-434f-b431-f115706dad70)

2. **Describe Resources**:
    ```sh
    kubectl describe pod <pod-name> -n hello-world
    kubectl describe svc hello-world-svc -n hello-world
    kubectl describe ingress hello-world-ingress -n hello-world
    ```
    These commands provide detailed information about the specified resources, which can help in diagnosing issues.

3. **Check Ingress Controller Logs**:
    If you are using an NGINX ingress controller, check its logs for any errors:
    ```sh
    kubectl logs <nginx-ingress-controller-pod> -n <ingress-controller-namespace>
    ```
4. **Validate YAML Files**:
    Ensure that the YAML files are correctly formatted and do not contain any syntax errors. You can use tools like `kubectl apply --dry-run=client -f <file>` to validate the files.

