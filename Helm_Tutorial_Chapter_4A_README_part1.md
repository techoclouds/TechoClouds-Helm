
# Helm Tutorial

## **Chapter 4A: Customizing Helm Charts - Basic Examples**

### **4A.1 Introduction to Helm Templates**

**Overview of Helm Templates**
Helm uses the Go templating language to dynamically generate Kubernetes manifests. Templates allow you to define Kubernetes resources with placeholders that can be replaced with actual values at deployment time.

**Understanding Variables in Helm Templates**

**Variable Types, Data Types, and Syntax**

In Helm templates, variables allow you to inject dynamic content into your Kubernetes manifests. Variables are sourced from `values.yaml` or other Helm objects, and they can represent various data types.

**Basic Variable Syntax**:
- **Accessing a value**: The syntax `{{ .Values.key }}` is used to access a specific key from `values.yaml`.
  ```yaml
  replicas: {{ .Values.replicaCount }}
  ```

- **Accessing nested values**: For nested keys, you chain them using the `.` (dot) notation.
  ```yaml
  image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
  ```

**Data Types in Helm Templates**:
- **String**: A sequence of characters.
  - **Example**: `{{ .Values.image.repository }}` might return `"nginx"`.
  - **Usage in template**:
    ```yaml
    image: "{{ .Values.image.repository }}"
    ```

- **Integer**: Numeric values without decimals.
  - **Example**: `{{ .Values.replicaCount }}` might return `3`.
  - **Usage in template**:
    ```yaml
    replicas: {{ .Values.replicaCount }}
    ```

- **Boolean**: True or False values.
  - **Example**: `{{ .Values.ingress.enabled }}` might return `true`.
  - **Usage in template**:
    ```yaml
    {{ if .Values.ingress.enabled }}
    ```

- **List/Array**: A list of items, often represented as a sequence of strings or maps.
  - **Example**: `{{ .Values.env }}` might return:
    ```yaml
    env:
      - name: APP_ENV
        value: production
      - name: LOG_LEVEL
        value: info
    ```
  - **Usage in template**:
    ```yaml
    env:
    {{ range .Values.env }}
      - name: {{ .name }}
        value: "{{ .value }}"
    {{ end }}
    ```

- **Map**: A collection of key-value pairs, similar to dictionaries in programming.
  - **Example**: `{{ .Values.config }}` might return:
    ```yaml
    config:
      database: mysql
      cache: redis
    ```
  - **Usage in template**:
    ```yaml
    data:
    {{ range $key, $value := .Values.config }}
      {{ $key }}: "{{ $value }}"
    {{ end }}
    ```

**Common Variable Types**:
- **.Values**: This represents the values defined in `values.yaml` and is the most commonly used variable type in Helm templates.
  - Example: `{{ .Values.replicaCount }}` accesses the `replicaCount` value from `values.yaml`.
  
- **.Release**: This contains information about the Helm release, such as its name.
  - Example: `{{ .Release.Name }}` is used to dynamically name resources based on the release name.
  
- **.Chart**: This contains metadata about the chart, like its name and version.
  - Example: `{{ .Chart.Name }}` can be used to reference the chart's name.

- **.Files**: This allows access to non-templated files within the chart.
  - Example: `{{ .Files.Get "file_name" }}` reads the content of a file in the chart.

These variables and data types provide a powerful way to customize your Kubernetes resources dynamically. Understanding the type of data you are working with is crucial for correctly structuring your Helm templates.

### **4A.2 Setting Up Your Environment**

**Step 1: Create a Working Directory**
1. Open your terminal.
2. Create a new directory for your Helm customization work:
   ```bash
   mkdir helm-custom
   cd helm-custom
   ```

**Step 2: Download a Helm Chart**
1. Use the `helm pull` command to download a chart for customization:
   ```bash
   helm pull bitnami/nginx --untar
   ```
   This will download and unpack the Nginx Helm chart into a directory named `nginx`.

**Step 3: Explore the Chart Structure**
1. List the contents of the `nginx` directory:
   ```bash
   ls nginx
   ```
   You'll see files like `Chart.yaml`, `values.yaml`, and a directory named `templates`.

2. Navigate to the `templates` directory, where the Kubernetes manifests are defined:
   ```bash
   cd nginx/templates
   ```

# Helm Template Customizations

## 4A.3 Using Variables in Helm Templates

### Defining and Accessing Variables

In Helm, variables are used to inject values from the `values.yaml` file into your templates. The most common way to access a value is using the `{{ .Values.key }}` syntax.

### Step 1: Modify the Deployment Template

Open the `deployment.yaml` file in the `nginx/templates` directory using your preferred text editor:

```bash
nano deployment.yaml
```

Find the section where `replicaCount` and `image` are defined. Replace the hardcoded values with variables from `values.yaml`:

```yaml
spec:
  replicas: {{ .Values.replicaCount }}
  containers:
  - name: {{ .Chart.Name }}
    image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

Save and close the file.

### Step 2: Modify the ConfigMap Template

Open the `configmap.yaml` file or create a new one if it doesn't exist:

```bash
nano configmap.yaml
```

Add the following content to define a ConfigMap using variables:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-config
data:
  config.yaml: |
    server:
      host: {{ .Values.server.host }}
      port: {{ .Values.server.port }}
```

Save and close the file.

## 4A.4 Conditionals in Helm Templates

### Introduction to Conditionals

Conditionals allow you to include or exclude parts of your template based on specific conditions. The basic syntax uses `{{ if }}`, `{{ else }}`, and `{{ end }}`.

### Step 1: Add Conditional Logic for Ingress

Open the `ingress.yaml` file in the `nginx/templates` directory or create a new one:

```bash
nano ingress.yaml
```

Add the following content to conditionally include the ingress resource:

```yaml
{{ if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}-ingress
spec:
  rules:
  - host: {{ .Values.ingress.host }}
{{ end }}
```

Save and close the file.

### Step 2: Add Conditional Logic for a Service

Open the `service.yaml` file or create a new one:

```bash
nano service.yaml
```

Add the following content to conditionally include the service resource:

```yaml
{{ if .Values.service.enabled }}
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-service
spec:
  ports:
  - port: 80
{{ end }}
```

Save and close the file.

## 4A.5 Loops in Helm Templates

### Introduction to Loops

Loops allow you to iterate over lists or maps in your `values.yaml` file. This is useful for generating multiple similar resources based on a list of values.

### Step 1: Iterate Over a List of Environment Variables

Open the `deployment.yaml` file and find the environment variables section:

```bash
nano deployment.yaml
```

Replace the existing environment variables with a loop to dynamically generate them:

```yaml
env:
{{ range .Values.env }}
  - name: {{ .name }}
    value: "{{ .value }}"
{{ end }}
```

Save and close the file.

### Step 2: Loop Over a Map in a ConfigMap

Open the `configmap.yaml` file:

```bash
nano configmap.yaml
```

Modify it to loop over a map of key-value pairs:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-config
data:
{{ range $key, $value := .Values.config }}
  {{ $key }}: "{{ $value }}"
{{ end }}
```

Save and close the file.

## 4A.6 Hands-On Exercise: Basic Customizations

### Modify and Deploy the Chart

- **Task**: Customize the `nginx` chart by modifying `values.yaml` to set your preferred replica count, image tag, and other settings.
- **Command**:

  ```bash
  helm install my-nginx ./nginx -f values.yaml
  ```

### Test Conditional Logic

- **Task**: Test the conditional logic by toggling the `ingress.enabled` and `service.enabled` values in `values.yaml` and deploying the chart. Observe the changes in the resources created.

### Use Loops to Generate Multiple Resources

- **Task**: Add multiple environment variables and ConfigMap entries in `values.yaml`, then deploy the chart and verify that the resources are generated correctly.