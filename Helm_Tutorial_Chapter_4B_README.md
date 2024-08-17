
# Helm Tutorial

## **Chapter 4B: Customizing Helm Charts - Advanced Examples**

### **4B.1 Setting Up Your Custom Helm Chart**

**Step 1: Create a New Helm Chart**
1. Open your terminal.
2. Create a new Helm chart named `custom-app`:
   ```bash
   helm create custom-app
   ```
   This command creates a new directory called `custom-app` with the basic structure of a Helm chart.

**Step 2: Explore the Chart Structure**
1. Navigate to the `templates` directory inside your `custom-app` chart:
   ```bash
   cd custom-app/templates
   ```
2. This is where you'll add and modify templates to implement the advanced examples.

### **4B.2 Implementing Advanced Examples in Your Chart**

#### **Nested Variables**

**Step 1: Modify the Deployment Template**
1. Open the `deployment.yaml` file in the `templates` directory:
   ```bash
   nano deployment.yaml
   ```
2. Replace the existing content with the following template using nested variables:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: {{ .Release.Name }}-app
   spec:
     replicas: {{ .Values.app.frontend.replicas }}
     template:
       spec:
         containers:
         - name: frontend
           image: "{{ .Values.app.frontend.image.repository }}:{{ .Values.app.frontend.image.tag }}"
         - name: backend
           image: "{{ .Values.app.backend.image.repository }}:{{ .Values.app.backend.image.tag }}"
   ```

**Step 2: Update `values.yaml`**
1. Open the `values.yaml` file in the root of the `custom-app` directory:
   ```bash
   nano ../values.yaml
   ```
2. Add the following nested structure to define the frontend and backend components:
   ```yaml
   app:
     frontend:
       replicas: 3
       image:
         repository: nginx
         tag: stable
     backend:
       image:
         repository: my-backend
         tag: 1.0.0
   ```

**Step 3: Test the Chart**
1. Deploy your custom chart with the nested variables:
   ```bash
   helm install my-app ../custom-app -f ../values.yaml
   ```
2. Verify that the deployment has created the expected resources:
   ```bash
   kubectl get deployments
   ```

#### **Complex Conditionals**

**Step 1: Add a Conditional Ingress Resource**
1. In the `templates` directory, create a new file named `ingress.yaml`:
   ```bash
   nano ingress.yaml
   ```
2. Add the following content to implement conditional logic:
   ```yaml
   {{- if and (eq .Values.env "production") .Values.enableIngress }}
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: {{ .Release.Name }}-ingress
   spec:
     rules:
     - host: {{ .Values.ingress.host }}
   {{- else if eq .Values.env "staging" }}
   apiVersion: v1
   kind: Service
   metadata:
     name: {{ .Release.Name }}-staging-service
   spec:
     ports:
     - port: 8080
   {{- end }}
   ```

**Step 2: Update `values.yaml`**
1. Add environment-specific settings to `values.yaml`:
   ```yaml
   env: production
   enableIngress: true
   ingress:
     host: "example.com"
   ```

**Step 3: Test the Chart**
1. Deploy your chart with the environment-specific configuration:
   ```bash
   helm install my-app ../custom-app -f ../values.yaml
   ```
2. Verify the creation of the Ingress resource:
   ```bash
   kubectl get ingress
   ```

#### **Advanced Looping Techniques**

**Step 1: Add a ConfigMap Template**
1. In the `templates` directory, create a new file named `configmaps.yaml`:
   ```bash
   nano configmaps.yaml
   ```
2. Add the following content to generate multiple ConfigMaps using a loop:
   ```yaml
   {{- range $key, $config := .Values.configs }}
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: {{ .Release.Name }}-{{ $key }}-config
   data:
   {{- range $config }}
     {{ .name }}: "{{ .value }}"
   {{- end }}
   {{- end }}
   ```

**Step 2: Update `values.yaml`**
1. Add the configuration data to `values.yaml`:
   ```yaml
   configs:
     app1:
       - name: LOG_LEVEL
         value: debug
       - name: DB_HOST
         value: db1.example.com
     app2:
       - name: LOG_LEVEL
         value: info
       - name: DB_HOST
         value: db2.example.com
   ```

**Step 3: Test the Chart**
1. Deploy your chart and check that the ConfigMaps have been created as expected:
   ```bash
   helm install my-app ../custom-app -f ../values.yaml
   ```
2. Verify the ConfigMaps:
   ```bash
   kubectl get configmaps
   ```

### **4B.3 Hands-On Exercise: Advanced Customizations**

1. **Refine the Chart**:
   - **Task**: Implement additional customizations based on your project’s requirements, using the advanced techniques covered in this chapter.
   - **Outcome**: Gain confidence in creating complex, flexible Helm charts that meet specific deployment needs.

2. **Deploy and Validate**:
   - **Task**: Deploy the chart in different environments (e.g., development, staging, production) and validate that the resources are created correctly based on the conditions and loops.
   - **Outcome**: Ensure that your chart is robust and adaptable to various deployment scenarios.

---

**Summary:**
In this chapter, you created a custom Helm chart and implemented advanced template techniques such as nested variables, complex conditionals, and looping. By testing these examples, you gained hands-on experience with building sophisticated Helm charts that can handle diverse and complex deployment scenarios.
