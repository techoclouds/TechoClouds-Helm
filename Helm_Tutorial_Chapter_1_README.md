
# Helm Tutorial

## **Chapter 1: Introduction to Helm**

### **1.1 Overview of Helm**

**What is Helm?**
Helm is a package manager for Kubernetes that simplifies the deployment, management, and maintenance of applications within Kubernetes clusters. It allows you to define, install, and upgrade even the most complex Kubernetes applications using charts, which are packages of pre-configured Kubernetes resources.

**Why Use Helm in Kubernetes?**
- **Simplifies Deployment:** Helm packages (charts) contain all the Kubernetes manifests you need to deploy an application.
- **Version Control:** Helm tracks the history of deployments, making it easy to roll back to previous versions.
- **Reuse and Share:** Helm charts can be reused and shared across different environments or teams, promoting consistency.
- **Parameterization:** You can customize applications using Helm by overriding default values in charts.

### **1.2 Understanding Helm Components**

**Charts**: A Helm chart is a collection of files that describe a related set of Kubernetes resources. It includes templates for the resources, default configuration values (`values.yaml`), and metadata about the chart.

**Repositories**: Helm charts are stored in repositories, which are like app stores for Helm. Popular repositories include the official Helm repository, Bitnami, and others.

**Releases**: When you install a Helm chart, it creates a release. A release is a deployed instance of a chart with a specific configuration.

### **1.3 Setting Up Helm**

**Step 1: Install Helm on Your Local Machine**

1. **For macOS**:
   ```bash
   brew install helm
   ```

2. **For Windows**:
   Download the installer from the [official Helm GitHub releases](https://github.com/helm/helm/releases) page and follow the installation instructions.

3. **For Linux (Ubuntu)**:
   ```bash
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```
   This command downloads and installs Helm on your Ubuntu system.

   **Alternatively, you can use the apt package manager:**
   ```bash
   sudo snap install helm --classic
   ```
   This command installs Helm via the Snap package manager.

**Step 2: Verify the Installation**
   ```bash
   helm version
   ```
   You should see the Helm version output, confirming the installation.

### **1.4 Hands-On Exercise: Installing Helm**

1. **Verify Helm Installation**: Run the `helm version` command to ensure that Helm is installed correctly.
   
2. **Explore Helm Commands**: Try running `helm help` to explore the various commands available in Helm.

3. **Add a Helm Repository**:
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   ```
   This command adds the Bitnami repository to your Helm configuration, which contains many popular charts.

4. **Search for a Chart**:
   ```bash
   helm search repo bitnami
   ```
   This command lists the available charts in the Bitnami repository.

---

**Summary:**
In this chapter, you've been introduced to Helm, its key components, and why it's valuable in Kubernetes. You've also set up Helm on your local machine and explored some basic commands to get started.
