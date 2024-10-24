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