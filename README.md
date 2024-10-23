# Amazon Elastic Kubernetes Service (EKS)

## Overview
Amazon Elastic Kubernetes Service (Amazon EKS) is a managed Kubernetes service that makes it easy to run Kubernetes on AWS without needing to install and operate your own Kubernetes control plane or worker nodes. EKS helps simplify Kubernetes deployment and management while providing scalability, security, and integration with AWS services.

## Architecture
### Control Plane
- **Managed by AWS**: The EKS control plane consists of multiple Kubernetes API servers and `etcd` nodes, spread across multiple AWS Availability Zones for high availability.
- **Responsibility**: AWS handles the operational tasks like control plane scaling, patching, and backup.

### Worker Nodes
- **EC2 Instances or Fargate**: Worker nodes are either EC2 instances or AWS Fargate, which run the application pods. You can use managed node groups (AWS manages nodes) or self-managed nodes.
- **Communication**: Worker nodes communicate with the control plane to manage and schedule pods.

### Networking
- **Amazon VPC CNI Plugin**: Pods are assigned IP addresses from your VPC, allowing secure communication with AWS services and within the cluster.
  
### Storage
- Integrate with AWS storage options like:
  - **Amazon EBS** (Elastic Block Store)
  - **Amazon EFS** (Elastic File System)
  - **S3** for object storage

## Key Components
### Amazon EKS Cluster
- A Kubernetes control plane with worker nodes that run workloads.

### Node Groups
- Collections of worker nodes (EC2 instances) used for running applications. Managed node groups allow AWS to handle updates and scaling.

### Fargate for EKS
- Serverless compute option for Kubernetes pods where AWS handles the underlying infrastructure.

### EKS Add-ons
- Pre-installed components such as CoreDNS, kube-proxy, and the AWS VPC CNI plugin that AWS manages.

### Bottlerocket
- A Linux-based OS optimized for running containers, offering enhanced security and faster updates.

## Features
### Fully Managed Control Plane
- AWS manages the Kubernetes control plane, including provisioning, scaling, and patching.

### Node Group Management
- Support for automatic scaling and patching of EC2 instances running worker nodes.

### Fargate Integration
- Run Kubernetes pods without provisioning EC2 instances, with compute resources automatically allocated by AWS.

### IAM Integration
- Integrate Kubernetes service accounts with AWS IAM roles to control access to AWS resources securely.

### Autoscaling
- Use Kubernetes Horizontal Pod Autoscaler (HPA) to automatically scale pods and Cluster Autoscaler to adjust worker nodes based on resource demands.

### App Mesh Integration
- Supports AWS App Mesh for controlling communication between services.

### AWS Add-ons
- Pre-installed components like CoreDNS and VPC CNI plugin, which AWS manages and updates.

### VPC Networking
- Each pod gets its own IP address within the VPC, integrating seamlessly with AWS services.

### Logging and Monitoring
- EKS integrates with **Amazon CloudWatch** for monitoring and logging control plane logs.

## Security
### IAM Roles for Service Accounts
- Map Kubernetes service accounts to IAM roles for fine-grained permissions.

### VPC Isolation
- EKS clusters operate inside a VPC, isolating network traffic and ensuring security.

### Encryption
- Use **KMS** to encrypt Kubernetes secrets, EBS volumes, and other sensitive data.

### Control Plane Logging
- Send control plane logs to CloudWatch for monitoring and security audits.

### Private Clusters
- You can configure private EKS clusters where the Kubernetes API is not exposed to the internet.

## Pricing
- **Control Plane**: $0.10 per hour per EKS cluster.
- **Worker Nodes**: Standard EC2 instance pricing. If using Fargate, you are charged based on vCPU and memory used by the pods.

## Setting up an EKS Cluster
### Step 1: Create the EKS Cluster
- Use the AWS Management Console, `eksctl`, or CloudFormation to create the control plane.

### Step 2: Provision Worker Nodes
- Create worker nodes using Managed Node Groups or EC2 instances. Alternatively, use Fargate profiles for serverless workloads.

### Step 3: Connect to the Cluster
- Use `kubectl` or `eksctl` to interact with the cluster and manage workloads.

### Step 4: Deploy Applications
- Deploy Kubernetes applications using manifests (`yaml` files).

## Common Use Cases
- **Microservices Architecture**: Deploy and scale microservices independently using Kubernetes.
- **CI/CD Pipelines**: Integrate with Jenkins, GitLab CI, AWS CodePipeline, and others for continuous deployment.
- **Hybrid Deployments**: Use AWS Outposts to run EKS clusters on-premises or in a hybrid cloud environment.
- **Big Data and Machine Learning**: Run workloads like Apache Spark or TensorFlow for data processing and machine learning.

## Best Practices
- **Use IAM Roles for Service Accounts**: Avoid using instance roles and map service accounts to IAM roles for security.
- **Monitoring and Logging**: Set up monitoring using CloudWatch, Prometheus, or Grafana for complete observability.
- **Security**: Regularly upgrade Kubernetes versions and apply best security practices to your EKS clusters.

## Alternatives
- **Amazon ECS**: Use for simpler container orchestration without Kubernetes.
- **Self-managed Kubernetes**: Install and manage Kubernetes clusters yourself on EC2 or on-premise infrastructure.

## Conclusion
Amazon EKS simplifies Kubernetes operations by automating tasks such as control plane management, scaling, and security integration with AWS services. With powerful autoscaling, security features, and integrations, EKS is a robust solution for running containerized applications on AWS.
