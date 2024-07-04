
# Test Assignment K8: Kubernetes and Monitoring

## Objective
To evaluate the candidate's experience with Kubernetes, including deployment, scaling, and monitoring.

## Task Description

### EKS Cluster Setup

1. **Set up an Amazon EKS (Elastic Kubernetes Service) cluster.**

### Deploy a Sample Application

2. **Deploy a sample application to the EKS cluster.**

### Ingress Controller

3. **Set up an Ingress Controller to manage external access to the services running in the cluster.**

### Horizontal Pod Autoscaler (HPA)

4. **Implement HPA based on memory utilization for the deployed application.**

### Monitoring

5. **Integrate Datadog for monitoring the Kubernetes cluster and the application.**
   - **Create dashboards and alerts in Datadog for critical metrics.**

### Documentation

6. **Document the steps to set up the EKS cluster, deploy the application, configure Ingress, and set up HPA.**
   - **Provide instructions for integrating Datadog and accessing monitoring dashboards.**

## Evaluation Criteria

- Ability to set up and manage Kubernetes clusters.
- Proficiency in deploying applications to Kubernetes.
- Experience with configuring Ingress Controllers and Horizontal Pod Autoscalers.
- Understanding of monitoring with Datadog.
- Quality of documentation and setup instructions.

We look forward to reviewing your submission. Should you have any questions, please do not hesitate to contact us.

---

## Instructions for the Assignment

### 1. EKS Cluster Setup

#### Step-by-Step Instructions

1. **Create an EKS Cluster:**
   - Sign in to the AWS Management Console.
   - Navigate to the EKS service.
   - Click on "Create cluster" and follow the wizard to set up a new EKS cluster.
   - Ensure you select appropriate settings for your use case (e.g., region, VPC, subnets).

2. **Configure kubectl for EKS:**
   - Follow AWS documentation to configure `kubectl` for EKS.
   - Test the configuration by running `kubectl get nodes`.

   \```bash
   aws eks --region eu-north-1 update-kubeconfig --name omega
   \```

### 2. Deploy a Sample Application

#### Step-by-Step Instructions

1. **Deploy a Sample Application:**
   - Create a deployment YAML file for a sample application (e.g., nginx).
   - Apply the deployment using `kubectl apply -f deployment.yaml`.

   \```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: omega-deployment
     labels:
       app: omega
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: omega
     template:
       metadata:
         labels:
           app: omega
       spec:
         containers:
         - name: omega
           image: chinnuworkspace/omega:latest
           ports:
           - containerPort: 3000
   \```

   \```bash
   kubectl apply -f deployment.yaml
   \```

   - Create a service YAML file for the sample application.

   \```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: omega-service
   spec:
     type: LoadBalancer
     ports:
       - port: 80
         targetPort: 3000
     selector:
       app: omega
   \```

   \```bash
   kubectl apply -f service.yaml
   \```

   - Get the services and wait for the external IP.

   \```bash
   kubectl get services
   \```

### 3. Ingress Controller

#### Step-by-Step Instructions

1. **Set up an Ingress Controller:**
   - Create an ingress YAML file for the sample application.

   \```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: omega-ingress
     annotations:
       nginx.ingress.kubernetes.io/rewrite-target: /
   spec:
     rules:
     - http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: omega-service
               port:
                 number: 3000
   \```

   \```bash
   kubectl apply -f ingress.yaml
   \```

### 4. Horizontal Pod Autoscaler (HPA)

#### Step-by-Step Instructions

1. **Implement HPA:**
   - Create an HPA configuration YAML file based on memory utilization.

   \```yaml
   apiVersion: autoscaling/v2
   kind: HorizontalPodAutoscaler
   metadata:
     name: omega-hpa
   spec:
     scaleTargetRef:
       apiVersion: apps/v1
       kind: Deployment
       name: omega-deployment
     minReplicas: 1
     maxReplicas: 10
     metrics:
     - type: Resource
       resource:
         name: memory
         target:
           type: Utilization
           averageUtilization: 50
   \```

   \```bash
   kubectl apply -f hpa.yaml
   \```

### 5. Monitoring with Datadog

#### Step-by-Step Instructions

1. **Integrate Datadog:**
   - Sign in to Datadog and obtain the API key.
   - Deploy the Datadog agent to the EKS cluster using Helm or a YAML file.
   - Configure the agent with the necessary permissions and the API key.

2. **Create Dashboards and Alerts:**
   - In Datadog, create a dashboard to monitor critical metrics of the Kubernetes cluster and the sample application.
   - Set up alerts for key metrics (e.g., high memory usage, pod restarts).

### 6. Documentation

#### Step-by-Step Instructions

1. **Document Setup and Configuration:**
   - Provide detailed documentation for each step, including commands and configurations used.
   - Include screenshots or command outputs where applicable.
   - Ensure the documentation is clear and easy to follow.

2. **Integrating Datadog:**
   - Document the process of integrating Datadog with the EKS cluster.
   - Provide instructions on how to access and use the monitoring dashboards in Datadog.

---

## Screenshots and Outputs

### EKS Cluster Setup

![EKS Cluster Creation](file-gQDrPcUp7RQA9pQRrA7rH86m)

### Node Configuration

![Node Configuration](file-dYDxl8hArpF1bYbniZlb27rX)

### Sample Application Deployment

![Sample Application Deployment](file-PlO0IKgfQFncgZNiQJAsTx9U)

### Ingress and HPA Setup

![Ingress and HPA Setup](file-ezaS7JwzyhI9TGchTTnesUHy)

### Pod and Service Status

![Pod and Service Status](file-bkJpSpxHDAyAJ13kSbkk8nAg)

### Application Access

![Application Access](file-9OO6N8HE59VHaLV1eEAj2gbo)

---

This completes the test assignment documentation. If you encounter any issues or have questions, please do not hesitate to contact us. Good luck!
