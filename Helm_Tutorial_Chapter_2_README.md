
# Helm Tutorial

## **Chapter 2: Using Helm Charts**

### **2.1 Exploring Helm Repositories**

**What Are Helm Repositories?**
Helm repositories are collections of packaged charts that can be easily downloaded and installed. Think of them as app stores specifically for Kubernetes applications. Popular repositories include the official Helm repository, Bitnami, and other community-maintained repositories.

**Adding a Helm Repository**
To use charts from a repository, you first need to add the repository to your Helm configuration.

- **Command**:
  ```bash
  helm repo add <repository_name> <repository_url>
  ```

- **Example**:
  To add the Bitnami repository:
  ```bash
  helm repo add bitnami https://charts.bitnami.com/bitnami
  ```

**Updating Helm Repositories**
To ensure you have the latest versions of charts available in your repositories, you need to update your Helm repositories.

- **Command**:
  ```bash
  helm repo update
  ```

### **2.2 Installing a Helm Chart**

**Searching for a Chart**
Before installing a chart, you can search for it in your added repositories.

- **Command**:
  ```bash
  helm search repo <chart_name>
  ```

- **Example**:
  To search for the Nginx chart in the Bitnami repository:
  ```bash
  helm search repo bitnami/nginx
  ```

**Installing a Chart**
Once you find the chart you want to install, you can deploy it to your Kubernetes cluster.

- **Command**:
  ```bash
  helm install <release_name> <chart_name>
  ```

- **Example**:
  To install the Nginx chart:
  ```bash
  helm install my-nginx bitnami/nginx
  ```

### **2.3 Understanding Helm Values**

**What Are Helm Values?**
Helm values are configuration options that you can pass to customize the behavior of a chart. These values are typically stored in a `values.yaml` file within the chart but can also be overridden at install time.

**Overriding Values During Installation**
You can override default values by passing them as arguments when you install the chart.

- **Command**:
  ```bash
  helm install <release_name> <chart_name> --set <key>=<value>
  ```

- **Example**:
  To install Nginx with a custom replica count:
  ```bash
  helm install my-nginx bitnami/nginx --set replicaCount=2
  ```

**Using a Custom Values File**
Instead of setting values individually, you can create a custom values file and pass it during installation.

- **Command**:
  ```bash
  helm install <release_name> <chart_name> -f <values_file.yaml>
  ```

- **Example**:
  ```bash
  helm install my-nginx bitnami/nginx -f custom-values.yaml
  ```

### **2.4 Hands-On Exercise: Deploying a Sample Application with Helm**

1. **Add the Bitnami Repository**:
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   ```
   
2. **Update the Repository**:
   ```bash
   helm repo update
   ```

3. **Search for the Nginx Chart**:
   ```bash
   helm search repo bitnami/nginx
   ```
   
4. **Install the Nginx Chart**:
   ```bash
   helm install my-nginx bitnami/nginx
   ```
   Verify that Nginx is running in your Kubernetes cluster.

5. **Customize the Nginx Installation**:
   - Install Nginx with a custom replica count:
     ```bash
     helm install my-nginx bitnami/nginx --set replicaCount=3
     ```

   - Alternatively, create a `custom-values.yaml` file with custom settings and install using:
     ```bash
     helm install my-nginx bitnami/nginx -f custom-values.yaml
     ```

6. **Verify the Installation**:
   Use `kubectl get pods` to verify that the Nginx pods are running and `kubectl get svc` to check the service.

---

**Summary:**
In this chapter, you've learned how to explore Helm repositories, search for and install Helm charts, and customize installations using Helm values. The hands-on exercise guided you through deploying and customizing a simple Nginx application in your Kubernetes cluster.
