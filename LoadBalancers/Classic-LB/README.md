## Classic Load Balancer (CLB)

By default, when you create a service with `type: LoadBalancer`, EKS will create a Classic Load Balancer. CLBs are suitable for simple HTTP or TCP applications.

### Step 1: Deploy Your Application

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: node:14
        ports:
        - containerPort: 3000
```
### Step 2: Create a Service for CLB
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clb-service
  labels:
    app: my-app
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 3000
    protocol: TCP
  selector:
    app: my-app
```

