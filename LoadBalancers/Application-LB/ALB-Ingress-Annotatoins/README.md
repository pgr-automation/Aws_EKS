# ALB (Application Load Balancer) Ingress Annotations

When setting up an ALB (Application Load Balancer) Ingress for Kubernetes, the annotations define various configurations for the ALB, such as SSL settings, target group settings, and routing behavior. Below is a list of commonly used annotations for configuring an ALB ingress in Kubernetes:

### 1. General Annotations

- #  kubernetes.io/ingress.class:
 Specifies the Ingress controller to use. For ALB, set it to alb.
```bash
kubernetes.io/ingress.class: alb
```
