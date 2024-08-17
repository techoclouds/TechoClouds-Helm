
# Helm Tutorial

## **Chapter 3: Managing Helm Releases**

### **3.1 Helm Release Lifecycle**

**What is a Helm Release?**
A Helm release is an instance of a Helm chart deployed on a Kubernetes cluster. When you install a chart, Helm creates a release to manage the resources associated with that deployment. Each release is uniquely identified by its name within the Kubernetes namespace.

**Helm Release Lifecycle:**
- **Install**: Deploy a new release of a chart.
- **Upgrade**: Update an existing release to a new version of the chart or modify its configuration.
- **Rollback**: Revert a release to a previous version in case of issues.
- **Uninstall**: Remove a release and its associated resources from the cluster.

### **3.2 Installing and Managing Helm Releases**

**Installing a New Release**
- **Command**:
  ```bash
  helm install <release_name> <chart_name>
  ```
- **Example**:
  ```bash
  helm install my-nginx bitnami/nginx
  ```

**Listing Releases**
- **Command**:
  ```bash
  helm list
  ```
- **Example**:
  List all releases in the current namespace:
  ```bash
  helm list
  ```
  
  List all releases across all namespaces:
  ```bash
  helm list --all-namespaces
  ```

### **3.3 Upgrading and Rolling Back Releases**

**Upgrading a Release**
- **Command**:
  ```bash
  helm upgrade <release_name> <chart_name> --set <key>=<value>
  ```
- **Example**:
  To upgrade the Nginx release with a new replica count:
  ```bash
  helm upgrade my-nginx bitnami/nginx --set replicaCount=4
  ```

**Rolling Back a Release**
- **Command**:
  ```bash
  helm rollback <release_name> <revision_number>
  ```
- **Example**:
  To roll back the Nginx release to its first revision:
  ```bash
  helm rollback my-nginx 1
  ```

**Viewing Release History**
- **Command**:
  ```bash
  helm history <release_name>
  ```
- **Example**:
  View the history of the Nginx release:
  ```bash
  helm history my-nginx
  ```

### **3.4 Uninstalling a Release**

**Uninstalling a Release**
When a release is uninstalled, Helm removes all the Kubernetes resources associated with that release.

- **Command**:
  ```bash
  helm uninstall <release_name>
  ```
- **Example**:
  To uninstall the Nginx release:
  ```bash
  helm uninstall my-nginx
  ```

**Cleaning Up After Uninstall**
Sometimes, even after uninstalling a release, certain resources (e.g., persistent volume claims) might remain. You can manually clean them up using `kubectl`.

- **Example**:
  ```bash
  kubectl delete pvc <persistent_volume_claim_name>
  ```

### **3.5 Hands-On Exercise: Managing Helm Releases**

1. **Install a New Release**:
   - Install the Nginx chart:
     ```bash
     helm install my-nginx bitnami/nginx
     ```

2. **List the Installed Releases**:
   - List all releases in the current namespace:
     ```bash
     helm list
     ```

3. **Upgrade the Release**:
   - Upgrade the Nginx release to increase the replica count:
     ```bash
     helm upgrade my-nginx bitnami/nginx --set replicaCount=4
     ```

4. **View the Release History**:
   - Check the history of the Nginx release:
     ```bash
     helm history my-nginx
     ```

5. **Rollback the Release**:
   - Roll back the Nginx release to its first revision:
     ```bash
     helm rollback my-nginx 1
     ```

6. **Uninstall the Release**:
   - Uninstall the Nginx release and clean up any leftover resources:
     ```bash
     helm uninstall my-nginx
     ```

---

**Summary:**
In this chapter, you've learned about the lifecycle of Helm releases, including how to install, upgrade, roll back, and uninstall releases. The hands-on exercise guided you through managing a Helm release, providing practical experience with each stage of the release lifecycle.
