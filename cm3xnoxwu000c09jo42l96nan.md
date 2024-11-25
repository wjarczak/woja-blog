---
title: "k3s on Ubuntu 24.04 LTS"
seoTitle: "Install k3s on Ubuntu 24.04 LTS"
seoDescription: "Learn how to install K3s on Ubuntu 24.04 LTS for efficient single-node Kubernetes deployments"
datePublished: Mon Nov 25 2024 23:25:35 GMT+0000 (Coordinated Universal Time)
cuid: cm3xnoxwu000c09jo42l96nan
slug: k3s-on-ubuntu
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/y8TMoCzw87E/upload/2aa2f60fc7e7007b7a5214f8430a6e17.jpeg
tags: nginx, kubernetes, k3s, rancher, load-balancing

---

This guide outlines the steps to install K3s on an Ubuntu 24.04 LTS server with 8 vCPUs and 16GB of memory. It’s tailored for single-node setups but can be extended to multi-node environments.

### Server Requirements

* **Ubuntu version**: 24.04 LTS

* **CPU**: 8 vCPUs

* **Memory**: 16GB RAM

* **Storage**: At least 20GB of free disk space

* **Access**: Root or a user with sudo privileges

---

### Update the System

```bash
sudo apt update && sudo apt upgrade -y
```

### Install Prerequisites

Install necessary dependencies:

```bash
sudo apt install -y curl wget software-properties-common
```

### Install K3s

Use the official K3s installation script without Traefik:

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Change <code>INSTALL_K3S_NAME=NODE-NAME</code> to your desired node name</div>
</div>

```bash
sudo curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--disable=traefik" INSTALL_K3S_NAME=NODE-NAME K3S_KUBECONFIG_MODE="644" INSTALL_K3S_VERSION="v1.30.6+k3s1" sh -
```

The script will:

* Install the specified version of K3s (v1.30.6+k3s1).

* Configure it as a systemd service.

* Set up a lightweight Kubernetes distribution.

See the documentation: [k3s Installation Guide](https://docs.k3s.io/installation/configuration)

---

### Verify Installation

Check if K3s is running:

```bash
sudo systemctl status k3s
```

Expected output should include a status of **active (running)**.

---

### Access Kubernetes

By default, K3s installs `kubectl`. You can check its version to ensure proper installation:

```bash
kubectl version --client
```

---

### Get Node and Cluster Info

Verify the node and cluster status:

```bash
kubectl get nodes -o wide
kubectl cluster-info
```

---

### Setup Aliases

For convenience, add aliases [from my previous article](https://woja.hashnode.dev/kubernetes-cheat-sheet).

Apply the changes:

```bash
source ~/.bash_aliases
```

---

### Install NGINX Load Balancer Ingress Controller

Learn more about the [NGINX Ingress Controller](https://docs.nginx.com/nginx-ingress-controller/overview/design/).

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.3/deploy/static/provider/baremetal/deploy.yaml
```

Please check if your Kubernetes version is supported.  
We are using `k3s v1.30.6`, which is compatible with `Ingress NGINX v1.11.3`.

See the documentation: [NGINX Ingress Controller Installation Guide](https://kubernetes.github.io/ingress-nginx/deploy/?ref=blog.thenets.org#bare-metal-clusters).

---

### Create NGINX Load Balancer Ingress Controller

```bash
touch nginx-lb.yaml
```

Paste the content below into the file:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ingress-nginx-controller-loadbalancer
  namespace: ingress-nginx
spec:
  selector:
    app.kubernetes.io/component: controller
    app.kubernetes.io/instance: ingress-nginx
    app.kubernetes.io/name: ingress-nginx
  ports:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 80
    - name: https
      port: 443
      protocol: TCP
      targetPort: 443
  type: LoadBalancer
```

Apply the created file:

```bash
ka nginx-lb.yaml # with my aliases
kubectl apply -f nginx-lb.yaml # without aliases
```

---

### What’s Next?

Your K3s installation is complete! You can now deploy workloads, configure services, and explore Kubernetes.

Refer to the official [K3s documentation](https://k3s.io) for advanced configuration and multi-node setups.
