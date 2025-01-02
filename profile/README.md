![image](https://github.com/user-attachments/assets/907c9f54-872c-436f-bc3f-73ec58666a60)
This project contains following git repository
1) Eurekaserver
2) Gatewayserver
3) Movie-service
4) Theatre
5) Booking-sevice
6) Configserver
7) Autherisation server

**1. Prerequisites**
AWS Account with proper permissions.
A running Amazon EKS Cluster.
Installed CLI tools:
kubectl
eksctl
AWS CLI
Git
Containerized application stored in a Docker Registry (e.g., Amazon Elastic Container Registry (ECR)).

3. Pipeline Architecture
A typical pipeline includes:

Source Control: Store your code inGitHub

Build: Use AWS CodeBuild or Jenkins to build and package the application.
Image Registry: Push Docker images to Amazon ECR.
Deploy: Use AWS CodeDeploy with Kubernetes configurations to deploy to your EKS cluster.
3. Steps to Build the Pipeline
**Step 1:** Set Up the EKS Cluster
Create an EKS cluster:
bash
Copy code
eksctl create cluster --name my-cluster --region us-west-2 --nodes 3
Configure kubectl to interact with the cluster:
bash
Copy code
aws eks --region us-west-2 update-kubeconfig --name my-cluster

**Step 2:** Prepare the Application

Write your Dockerfile to containerize the application.
Create Kubernetes deployment and service YAML files (e.g., deployment.yaml, service.yaml).
Step 3: Set Up AWS CodePipeline
Create a pipeline using AWS Management Console or CLI:

bash
**Copy code**
aws codepipeline create-pipeline --cli-input-json file://pipeline-definition.json
Replace pipeline-definition.json with the configuration file defining the pipeline structure.

Define pipeline stages:

**Source:** Fetch the code from GitHub.

**Build:** Use AWS CodeBuild to build the Docker image and push it to Amazon ECR.

Deploy: Use a custom deployment script or CodeDeploy with your kubectl commands.
**Step 4:** Automate Docker Image Build and Push
Create a CodeBuild project with a buildspec.yml file:
yaml
Copy code
version: 0.2
phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin <ECR_URI>
  build:
    commands:
      - echo Building the Docker image...
      - docker build -t my-app .
      - docker tag my-app:latest <ECR_URI>:latest
  post_build:
    commands:
      - echo Pushing the Docker image...
      - docker push <ECR_URI>:latest
      - echo Build completed on `date`
Replace <ECR_URI> with your Amazon ECR repository URI.
**Step 5:** Deploy to Kubernetes
Use kubectl commands to apply Kubernetes configurations:
bash
Copy code
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
Automate this in the pipeline by adding a deploy stage that runs these commands.
4. Monitor and Optimize
Use AWS CloudWatch and AWS X-Ray for monitoring.
Enable Kubernetes metrics with Prometheus or Amazon Managed Service for Prometheus.


