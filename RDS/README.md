# Running AWS RDS MySQL with Kubernetes

This guide explains how to connect your Kubernetes applications to an external AWS RDS MySQL database instead of hosting the database within the Kubernetes cluster. By using AWS RDS, you offload database management tasks like backups, patching, and scaling, while Kubernetes handles your application layer.

## Steps to Integrate AWS RDS MySQL with Kubernetes

### 1. Create AWS RDS MySQL Instance
- Launch an Amazon RDS MySQL instance via the AWS Management Console, Terraform, or CloudFormation.
- Ensure that the RDS instance is in the same VPC as your Kubernetes cluster, or set up proper VPC peering or a VPN if they are in different VPCs.
- Configure security groups for the RDS instance to allow traffic from the Kubernetes worker nodes. Add the worker nodes' security group to the inbound rules of the RDS security group on port `3306`.

### 2. Obtain RDS Connection Details
Once the RDS instance is created, retrieve the following:
- **RDS endpoint** (host)
- **Port** (default: 3306)
- **Database name**
- **Username** and **Password**

### 3. Create Kubernetes Secrets
Store the database credentials securely in Kubernetes using Secrets:
```bash
kubectl create secret generic rds-mysql-secret \
  --from-literal=db-username=<your-username> \
  --from-literal=db-password=<your-password>
```
### 4. Create ConfigMap for RDS MySQL Endpoint

Store the RDS connection details in a Kubernetes ConfigMap:
```bash
kubectl create configmap rds-mysql-config \
  --from-literal=db-host=<rds-endpoint> \
  --from-literal=db-port=3306 \
  --from-literal=db-name=<database-name>
```

### 5. Update Your Kubernetes Application to Use RDS

Modify your Kubernetes application (e.g., deployment.yaml) to use the created Secrets and ConfigMap for database access:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: my-app-image
        env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: rds-mysql-config
              key: db-host
        - name: DB_PORT
          valueFrom:
            configMapKeyRef:
              name: rds-mysql-config
              key: db-port
        - name: DB_NAME
          valueFrom:
            configMapKeyRef:
              name: rds-mysql-config
              key: db-name
        - name: DB_USER
          valueFrom:
            secretKeyRef:
              name: rds-mysql-secret
              key: db-username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: rds-mysql-secret
              key: db-password
```

### 7. Optional: Set Up Service Mesh or Proxy for Secure Connections

- You can implement a service mesh (e.g., Istio, Linkerd) or use a sidecar proxy (e.g., Envoy) to secure and manage traffic between your Kubernetes pods and the RDS instance.

### 8. Monitoring and Scaling

- Use AWS CloudWatch to monitor your RDS instance.
- Enable Kubernetes Horizontal Pod Autoscaler to adjust application scaling based on load, while the RDS instance handles database scaling through AWS RDS capabilities.

### Best Practices

- Ensure RDS security groups allow access only from your Kubernetes worker nodes or specific IP ranges.
-  Use AWS Secrets Manager or HashiCorp Vault for secure management of database credentials, if possible.
- Configure VPC peering or private subnets for enhanced security and isolation.
- Enable RDS Performance Insights to monitor and optimize query performance.