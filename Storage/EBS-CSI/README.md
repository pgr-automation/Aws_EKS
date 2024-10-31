# Amazon EBS CSI Driver for Amazon EKS

The **Amazon EBS CSI (Elastic Block Store Container Storage Interface) driver** for Amazon EKS enables Kubernetes clusters to use Amazon EBS volumes as persistent storage. It simplifies the management of storage in EKS clusters by leveraging native EBS capabilities while adhering to the CSI standard.

## Features

1. **Dynamic Provisioning**: Automatically provisions EBS volumes based on the storage requirements specified in Kubernetes PersistentVolumeClaims (PVCs).
2. **Volume Snapshots**: Supports creating snapshots of EBS volumes, enabling backup and recovery.
3. **Volume Resizing**: Allows you to resize EBS volumes dynamically without downtime or manual intervention.
4. **Multi-AZ Support**: Ensures that EBS volumes can be attached to nodes in the same availability zone (AZ) as the volume.
5. **Volume Encryption**: Supports encryption for data at rest using AWS Key Management Service (KMS).
6. **Volume Types**: Supports different EBS volume types such as General Purpose SSD (gp2, gp3), Provisioned IOPS SSD (io1, io2), Throughput Optimized HDD (st1), and Cold HDD (sc1).

## Advantages

1. **Native Integration with AWS**: Seamlessly integrates with AWS services, ensuring that storage provisioning and management take advantage of AWS’s cloud capabilities.
2. **Automated Volume Management**: Automatically provisions and manages volumes based on Kubernetes specifications, reducing manual overhead.
3. **Scalability**: Easily scales as your workloads grow, providing highly available and durable storage.
4. **Cost Optimization**: By supporting different EBS volume types, you can choose cost-efficient storage based on your application’s IOPS and throughput requirements.
5. **Data Durability**: Provides data durability by leveraging AWS’s EBS storage, which replicates data within the same AZ.
6. **Backup and Restore**: With snapshot support, it’s easy to create backups of your volumes and restore them when necessary.

## Use Cases

1. **Stateful Applications**: Ideal for applications that require persistent storage, such as databases (MySQL, PostgreSQL, MongoDB), where each instance needs its own dedicated volume.
2. **Backup and Disaster Recovery**: Supports creating volume snapshots for backup, making it useful for disaster recovery solutions.
3. **Large Data Processing**: For applications that handle large datasets (e.g., analytics pipelines), EBS can provide the necessary throughput and IOPS.
4. **CI/CD Pipelines**: Useful in CI/CD systems where build artifacts or Docker images need to be stored on persistent volumes across deployments.
5. **Machine Learning Workloads**: Machine learning models and datasets can be stored on persistent EBS volumes, ensuring data persistence across model training sessions.
6. **Logs and Monitoring**: Storing logs or monitoring data from Prometheus or Elasticsearch requires persistent volumes that are durable and scalable.

## How to Use the EBS CSI Driver

## EKS Storage with EBS - Elastic Block Store

## Step-01: Introduction
- Create IAM Policy for EBS
- Associate IAM Policy to Worker Node IAM Role
- Install EBS CSI Driver

## Step-02:  Create IAM policyy
- Go to Services -> IAM
- Create a Policy 
  - Select JSON tab and copy paste the below JSON
```json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AttachVolume",
        "ec2:CreateSnapshot",
        "ec2:CreateTags",
        "ec2:CreateVolume",
        "ec2:DeleteSnapshot",
        "ec2:DeleteTags",
        "ec2:DeleteVolume",
        "ec2:DescribeInstances",
        "ec2:DescribeSnapshots",
        "ec2:DescribeTags",
        "ec2:DescribeVolumes",
        "ec2:DetachVolume"
      ],
      "Resource": "*"
    }
  ]
}
```
  - Review the same in **Visual Editor** 
  - Click on **Review Policy**
  - **Name:** Amazon_EBS_CSI_Driver
  - **Description:** Policy for EC2 Instances to access Elastic Block Store
  - Click on **Create Policy**

## Step-03: Get the IAM role Worker Nodes using and Associate this policy to that role
```
# Get Worker node IAM Role ARN
kubectl -n kube-system describe configmap aws-auth

# from output check rolearn
rolearn: arn:aws:iam::180789647333:role/eksctl-eksdemo1-nodegroup-eksdemo-NodeInstanceRole-IJN07ZKXAWNN
```
- Go to Services -> IAM -> Roles 
- Search for role with name **eksctl-eksdemo1-nodegroup** and open it
- Click on **Permissions** tab
- Click on **Attach Policies**
- Search for **Amazon_EBS_CSI_Driver** and click on **Attach Policy**

## Step-04: Deploy Amazon EBS CSI Driver  
- Verify kubectl version, it should be 1.14 or later
```
kubectl version --client --short
```
- Deploy Amazon EBS CSI Driver
```
# Deploy EBS CSI Driver
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
helm install aws-ebs-csi-driver aws-ebs-csi-driver/aws-ebs-csi-driver \
--namespace kube-system

# Verify ebs-csi pods running
kubectl get pods -n kube-system
```
