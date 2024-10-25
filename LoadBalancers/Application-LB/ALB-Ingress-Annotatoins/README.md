# ALB (Application Load Balancer) Ingress Annotations

When setting up an ALB (Application Load Balancer) Ingress for Kubernetes, the annotations define various configurations for the ALB, such as SSL settings, target group settings, and routing behavior. Below is a list of commonly used annotations for configuring an ALB ingress in Kubernetes:

### 1. General Annotations

-  kubernetes.io/ingress.class:
   Specifies the Ingress controller to use. For ALB, set it to alb.
```yaml
kubernetes.io/ingress.class: alb
```
- alb.ingress.kubernetes.io/scheme:
Defines the load balancer scheme. It can be internet-facing or internal.
```yaml
alb.ingress.kubernetes.io/scheme: internet-facing
```
- alb.ingress.kubernetes.io/target-type:
Specifies whether the targets are instance (EC2 instances) or ip (pods).
```yaml
alb.ingress.kubernetes.io/target-type: ip
```
- alb.ingress.kubernetes.io/listen-ports:
Defines the ports for the ALB. Example for HTTP and HTTPS:
```yaml
alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
```
### 2. Health Check Annotations
- alb.ingress.kubernetes.io/healthcheck-path:
Defines the path for the health check on the target group.
```yaml
alb.ingress.kubernetes.io/healthcheck-path: /health
```
- alb.ingress.kubernetes.io/healthcheck-interval-seconds:
Specifies the interval (in seconds) for the health check.
```yaml
alb.ingress.kubernetes.io/healthcheck-interval-seconds: '30'
```
- alb.ingress.kubernetes.io/healthcheck-timeout-seconds:
Sets the timeout for health checks.
```yaml
alb.ingress.kubernetes.io/healthcheck-timeout-seconds: '5'
```

### 3. SSL/TLS Annotations
- alb.ingress.kubernetes.io/certificate-arn:
Specifies the ARN of the SSL certificate to use for HTTPS listeners.
```yaml
alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:region:account-id:certificate/certificate-id
```
- alb.ingress.kubernetes.io/ssl-policy:
Defines the security policy for SSL. Example: ELBSecurityPolicy-2016-08.
```yaml
alb.ingress.kubernetes.io/ssl-policy: ELBSecurityPolicy-2016-08
```
- alb.ingress.kubernetes.io/ssl-redirect:
Redirects HTTP requests to HTTPS.
```yaml
alb.ingress.kubernetes.io/ssl-redirect: '443'
```
### 4. Target Group Annotations

- alb.ingress.kubernetes.io/success-codes:
Specifies the HTTP status codes that indicate a successful health check.
```yaml
alb.ingress.kubernetes.io/success-codes: '200,302'
```
- alb.ingress.kubernetes.io/target-group-attributes:
Allows you to specify additional attributes for the target group.
```yaml
alb.ingress.kubernetes.io/target-group-attributes: stickiness.enabled=true,stickiness.lb_cookie.duration_seconds=3600
```
### 5. WAF and Shield Annotations
- alb.ingress.kubernetes.io/waf-acl-id:
Specifies the Web Application Firewall (WAF) ACL ID to associate with the ALB.
```yaml
alb.ingress.kubernetes.io/waf-acl-id: waf-acl-id
```
- alb.ingress.kubernetes.io/shield-advanced-protection:
Enables AWS Shield Advanced protection for the ALB.
```yaml
alb.ingress.kubernetes.io/shield-advanced-protection: 'true'
```
### 6. Routing Annotations
- alb.ingress.kubernetes.io/load-balancer-attributes:
Custom attributes for the load balancer.
```yaml
alb.ingress.kubernetes.io/load-balancer-attributes: idle_timeout.timeout_seconds=60
```
- alb.ingress.kubernetes.io/actions.${service-name}:
Allows for custom routing actions. For example, routing to a weighted target group.
```yaml
alb.ingress.kubernetes.io/actions.my-service: '{"type":"forward","forwardConfig":{"targetGroups":[{"serviceName":"my-service","servicePort":"80","weight":100}]}}'
```

### 7. Security Group Annotations
- alb.ingress.kubernetes.io/security-groups:
Specifies the security groups to associate with the ALB.
```yaml
alb.ingress.kubernetes.io/security-groups: sg-12345678, sg-87654321
```
- alb.ingress.kubernetes.io/ip-address-type:
Specifies the IP address type for the ALB, either ipv4 or dualstack.
```yaml
alb.ingress.kubernetes.io/ip-address-type: dualstack
```
### 8. Access Logs Annotations
- alb.ingress.kubernetes.io/load-balancer-attributes (for access logs):
Configures access logs for the ALB
```yaml
alb.ingress.kubernetes.io/load-balancer-attributes: 'access_logs.s3.enabled=true,access_logs.s3.bucket=my-alb-logs,access_logs.s3.prefix=myapp'
```
### Example of a Full Ingress Resource for ALB:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: default
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:region:account-id:certificate/certificate-id
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
    alb.ingress.kubernetes.io/healthcheck-path: /health
    alb.ingress.kubernetes.io/success-codes: '200,302'
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```