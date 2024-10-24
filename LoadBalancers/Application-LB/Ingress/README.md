
# AWS ALB Ingress Overview

AWS ALB (Application Load Balancer) Ingress is a Kubernetes ingress controller that integrates with Amazon's Application Load Balancer (ALB). This setup allows you to route traffic from the Internet or other sources into your Kubernetes cluster, using the ALB as a proxy to manage routing for various services within the cluster.

## Key Features

- **Layer 7 Load Balancing**: ALB provides Layer 7 (HTTP/HTTPS) routing, making it ideal for web traffic. It can route traffic based on HTTP headers, paths, hostnames, and more.
- **SSL/TLS Termination**: ALB can offload SSL/TLS decryption, simplifying your workload.
- **Automatic Scaling**: ALB scales based on incoming traffic, ensuring high availability.
- **Integration with Kubernetes**: By using the ALB Ingress Controller, ALB can be provisioned and managed automatically based on Kubernetes Ingress resources.
- **Security**: ALB integrates with AWS security features like IAM roles, security groups, and Web Application Firewall (WAF) for enhanced security.

## How AWS ALB Ingress Works

### ALB Ingress Controller
The ALB Ingress Controller is the core component that runs inside your Kubernetes cluster. It monitors for Kubernetes Ingress resources and automatically configures the ALB based on the routing rules defined in those resources.

### Ingress Resource
An Ingress resource is a Kubernetes object that defines how external traffic should be routed to services within your cluster. The ALB Ingress Controller processes this resource and updates the ALB configuration accordingly.

### Ingress Class
An IngressClass resource allows you to specify which ingress controller to use (in this case, ALB). This enables Kubernetes to route traffic through ALB instead of using other ingress controllers.

## Example Usage

To deploy an ALB Ingress in your Kubernetes cluster, you would typically define an Ingress resource like this:


## Configure

### 1. Define an IngressClass
```yaml
apiVersion: networking.k8s.io/v1
kind: IngressClass
metadata:
  name: alb
  ingressclass.kubernetes.io/is-default-class: "true"
spec:
  controller: ingress.k8s.aws/alb
```

### 2.  Create an Ingress Resource
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-alb-ingress
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP":80}, {"HTTPS": 443}]'
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-app-service
            port:
              number: 80

```