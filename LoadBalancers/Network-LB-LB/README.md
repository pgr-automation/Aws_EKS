# Network Load Balancer (NLB)

NLBs handle millions of requests per second and operate at Layer 4 (TCP/UDP).
### Step 1: Create a Service for NLB
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nlb-service
  labels:
    app: my-app
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: "nlb"
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 3000
    protocol: TCP
  selector:
    app: my-app
```
