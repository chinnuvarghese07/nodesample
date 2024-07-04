
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

   ```bash
   aws eks --region eu-north-1 update-kubeconfig --name omega
   ```

### 2. Deploy a Sample Application

#### Step-by-Step Instructions

1. **Deploy a Sample Application:**
   - Create a deployment YAML file for a sample application (e.g., nginx).
   - Apply the deployment using `kubectl apply -f deployment.yaml`.

   ```yaml
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
   ```

   ```bash
   kubectl apply -f deployment.yaml
   ```

   - Create a service YAML file for the sample application.

   ```yaml
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
   ```

   ```bash
   kubectl apply -f service.yaml
   ```

   - Get the services and wait for the external IP.

   ```bash
   kubectl get services
   ```

### 3. Ingress Controller

#### Step-by-Step Instructions

1. **Set up an Ingress Controller:**
   - Create an ingress YAML file for the sample application.

   ```yaml
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
   ```

   ```bash
   kubectl apply -f ingress.yaml
   ```

### 4. Horizontal Pod Autoscaler (HPA)

#### Step-by-Step Instructions

1. **Implement HPA:**
   - Create an HPA configuration YAML file based on memory utilization.

   ```yaml
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
   ```

   ```bash
   kubectl apply -f hpa.yaml
   ```

### 5. Monitoring with Datadog

#### Step-by-Step Instructions

1. **Integrate Datadog:**
   - Sign in to Datadog and obtain the API key.
   - Configure the agent with the necessary permissions and the API key.
   - Integrate the AWS plugin in Datadog for monitoring the EKS cluster.
     ![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/24483705-6b0e-4c01-9ff7-29fda11d3022)


2. **Create Dashboards and Alerts:**
   - In Datadog, create a dashboard to monitor critical metrics of the Kubernetes cluster and the sample application.
   - Set up alerts for key metrics (e.g., high memory usage, pod restarts).

---

## Screenshots and Outputs


### EKS Cluster Setup

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/3a32c2c1-2938-4203-8fd1-44f46223057d)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/743bc99d-7f7e-4203-bddf-3f37a4b62054)



### Node Configuration

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/05b10a3a-e8b4-4273-8010-810ff296a6ff)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/596cf474-e7e6-43b5-b9cf-20361f8a7aba)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/713e08c7-1a08-40f0-bc5f-5ff362c482f7)



### Sample Application Deployment , Ingress and HPA Setup ,Pod and Service Status

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/0ba71b0a-4463-4699-9d26-e9b739104042)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/5297431f-db24-47f5-9250-4f715bc807ce)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/7ae91175-30ad-44a2-b6fc-c32136711bc7)



### Application Access

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/e6458ae5-5508-4a09-b53b-e90ddbfa41aa)



### Datadog

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/0cef7bd6-f4c7-474e-8868-a28f666b9be0)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/95b7b820-930e-40f2-a856-878d8cc6dba8)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/fe075608-34bf-41f8-86c5-1b97c2b899d6)


![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/eff0e97d-ee57-4cec-b81b-340dea5742c8)


![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/c8c045e5-283b-45c6-ad09-a9d0147ca7ce)

![image](https://github.com/chinnuvarghese07/nodesample/assets/11041542/dd8e21ce-bf57-43c6-8284-ccee2e8ddc92)

