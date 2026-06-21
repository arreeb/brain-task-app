Project Overview

This project demonstrates a fully automated CI/CD pipeline built entirely on AWS. It takes a pre-compiled React frontend application, containerizes it using an Nginx Docker image, pushes it to AWS Elastic Container Registry (ECR), and automatically deploys it to an Amazon Elastic Kubernetes Service (EKS) cluster using AWS CodePipeline.

Architecture & Pipeline Explanation :
The pipeline follows the below Continuous Integration and Continuous Deployment process:

Source Code Management :
The application code, Dockerfile, and Kubernetes manifest file are stored in a GitHub repository.

Continuous Integration (AWS CodeBuild) :
Whenever changes are pushed to the main branch, AWS CodePipeline automatically triggers AWS CodeBuild.
CodeBuild performs the following tasks -
Logs in to AWS ECR. 
Builds a new Docker image. 
Tags the Docker image. 
Pushes the image to the private ECR repository. 
In simple terms, CodeBuild acts as the build server that prepares the application image for deployment.

Continuous Deployment (AWS CodePipeline to EKS) :
After the build is completed successfully, the AWS CodePipeline Deploy to EKS action automatically reads the manifest.yaml file and updates the Kubernetes pods with the latest Docker image pushed to ECR.
This ensures that the latest version of the application is deployed automatically without any manual effort.

Hosting (Amazon EKS) :
The application is hosted on a 2-node AWS EKS cluster using t3.medium instances.
A Kubernetes Service of type LoadBalancer is used to expose the application to the internet on Port 3000.

Monitoring (Amazon CloudWatch) :
AWS CloudWatch is used for monitoring and logging.
Execution logs from CodeBuild, deployment logs from the EKS Control Plane, and application pod logs are tracked and viewable in AWS CloudWatch.

Setup Instructions :
To replicate this environment, the following infrastructure was provisioned via the AWS Console:

IAM Roles:
EKS-Cluster-Role for the Kubernetes Control Plane. 
EKS-Node-Role for the EC2 worker nodes (with ECR read permissions). 

Kubernetes Cluster:
Created an Amazon EKS Cluster named brain-tasks-cluster. 
Provisioned a managed Node Group with two t3.small instances.

Container Registry:
Created a private AWS ECR repository to store the Nginx/React Docker images. 
CI/CD Automation:
Created an AWS CodeBuild project (brain-tasks-build) running in privileged mode to build Docker images. 
Created an AWS CodePipeline (brain-tasks-pipeline) linking GitHub (V2), CodeBuild, and Amazon EKS. 

EKS Access Configuration:
Configured an EKS Access Entry granting the CodePipeline Service Role AmazonEKSClusterAdminPolicy so it could deploy to the cluster.

Project Screenshots -

1.Application Running on port 3000
<img width="952" height="477" alt="Application running on port 3000" src="https://github.com/user-attachments/assets/0a254fa7-d98e-451a-9e2f-234a65bd92bc" />

2.EKS cluster and Node Group
<img width="955" height="413" alt="EKS Cluster" src="https://github.com/user-attachments/assets/d11b8202-013c-4fa8-8634-8872b5ffd530" />
<img width="946" height="470" alt="EKS Worker Nodes" src="https://github.com/user-attachments/assets/b798ec40-3fe2-4657-9f0c-5f6fdbcbe6e2" />

3.Kubernetes Pods 
<img width="960" height="506" alt="K8 pods" src="https://github.com/user-attachments/assets/73b33337-f3a5-4755-9346-f963e8bdc064" />

4.AWS code build
<img width="950" height="512" alt="AWS CodeBuild" src="https://github.com/user-attachments/assets/20d59c7d-847c-4ad4-a425-f34d3dc3f838" />

5.ECR Private Repo
<img width="949" height="510" alt="ECR Private Repo" src="https://github.com/user-attachments/assets/69513d45-2ee8-4e9b-bb7a-82fe66bee299" />

6.CI/CD Pipeline
<img width="950" height="509" alt="CI_CD Pipeline Successfull" src="https://github.com/user-attachments/assets/a807ed30-856a-407a-a569-917bab22bdfc" />

7.AWS Cloudwatch Monitoring & Log
<img width="956" height="511" alt="Cloudwatch Log Management" src="https://github.com/user-attachments/assets/dc94301d-0695-4df4-9e62-4a26df3e5e2a" />
<img width="953" height="511" alt="Cloudwatch Log Management 2" src="https://github.com/user-attachments/assets/af98e9f9-45f5-4b93-8ac9-3378d6135153" />












