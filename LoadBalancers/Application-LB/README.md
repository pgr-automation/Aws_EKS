# Application LoadBalaner


### AWS Load Balancer Controller
![alt text](image.png)


## Steps:

- Create IAM Policy and make a note of Policy ARN
- Create IAM Role and k8s Service Account and bound them together
- Install AWS Load Balancer Controller using HELM3 CLI
- Understand IngressClass Concept and create a default Ingress Class

### 1. Create IAM Policy
```bash
https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json
```

```bash
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json 
```
```bash
prashanthg@prashanthg:~/automation/Aws_EKS/LoadBalancers/Application-LB$ aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json  
{
    "Policy": {
        "PolicyName": "AWSLoadBalancerControllerIAMPolicy",
        "PolicyId": "ANPA4MTWGX34KKO6MEQZH",
        "Arn": "arn:aws:iam::851725172472:policy/AWSLoadBalancerControllerIAMPolicy",
        "Path": "/",
        "DefaultVersionId": "v1",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 0,
        "IsAttachable": true,
        "CreateDate": "2024-10-24T20:48:16+00:00",
        "UpdateDate": "2024-10-24T20:48:16+00:00"
    }
}
```
### 2. Create IAM Role using eksctl
```bash
eksctl create iamserviceaccount \
  --cluster=my_cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \ #Note:  K8S Service Account Name that need to be bound to newly created IAM Role
  --attach-policy-arn=arn:aws:iam::851725172472:policy/AWSLoadBalancerControllerIAMPolic \
  --override-existing-serviceaccounts \
  --approve
  ```
  ```bash
kubectl get sa -n kube-system
kubectl get sa aws-load-balancer-controller -n kube-system
```