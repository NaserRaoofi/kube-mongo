# MongoDB & Mongo Express Kubernetes Deployment

A complete Kubernetes deployment setup for MongoDB database with Mongo Express web interface for database administration.

## 🚀 Features

- **MongoDB Database**: Production-ready MongoDB deployment with authentication
- **Mongo Express**: Web-based MongoDB administration interface
- **Secure Authentication**: Base64 encoded secrets for all credentials
- **Resource Management**: Proper CPU and memory limits
- **External Access**: NodePort service for web UI access
- **High Availability**: MongoDB deployment with 2 replicas

## 📋 Prerequisites

- Kubernetes cluster (Minikube, Docker Desktop, or cloud provider)
- `kubectl` command-line tool
- Basic knowledge of Kubernetes

## 🛠️ Quick Start

### 1. Clone the Repository

```bash
git clone git@github.com:NaserRaoofi/kube-mongo.git
cd kube-mongo
```

### 2. Deploy to Kubernetes

```bash
# Apply all manifests at once
kubectl apply -f .

# Or apply in order:
kubectl apply -f secret.yml
kubectl apply -f configmap.yml
kubectl apply -f deployment-mongo.yml
kubectl apply -f service-mongo.yml
kubectl apply -f deployment-expressmongo.yml
kubectl apply -f service-express.yml
```

### 3. Access Mongo Express

```bash
# Get Minikube IP (if using Minikube)
minikube ip

# Access via browser: http://MINIKUBE_IP:30081
# Login credentials: admin / admin123
```

## 📁 Project Structure

```
├── README.md                    # This file
├── secret.yml                   # MongoDB and Mongo Express credentials
├── configmap.yml               # Non-sensitive configuration
├── deployment-mongo.yml        # MongoDB deployment
├── service-mongo.yml          # MongoDB internal service
├── deployment-expressmongo.yml # Mongo Express deployment
└── service-express.yml        # Mongo Express NodePort service
```

## 🔧 Configuration

### MongoDB Credentials

The MongoDB uses the following credentials (base64 encoded in `secret.yml`):

- **Username**: `sirwan`
- **Password**: `sirwan123`

### Mongo Express UI Access

- **Username**: `admin`
- **Password**: `admin123`
- **Port**: `30081` (NodePort)

### Resource Limits

**MongoDB Container:**

- Memory: 256Mi - 512Mi
- CPU: 250m - 500m

**Mongo Express Container:**

- Memory: 128Mi - 256Mi
- CPU: 100m - 200m

## 🌐 Services

### MongoDB Service (Internal)

- **Type**: ClusterIP
- **Port**: 27017
- **Access**: Internal cluster only

### Mongo Express Service (External)

- **Type**: NodePort
- **Port**: 8081 → 30081
- **Access**: External via `http://NODE_IP:30081`

## 🔒 Security

- All sensitive credentials stored in Kubernetes secrets
- Base64 encoding for all secret values
- Separation of sensitive and non-sensitive configuration
- Basic authentication enabled for Mongo Express web interface

## 📊 Monitoring & Troubleshooting

### Check Pod Status

```bash
kubectl get pods
```

### View Logs

```bash
# MongoDB logs
kubectl logs deployment/mongo-deployment

# Mongo Express logs
kubectl logs deployment/mongo-express-deployment
```

### Verify Services

```bash
kubectl get services
```

### Port Forward (Alternative Access)

```bash
# Forward Mongo Express to localhost:8081
kubectl port-forward service/mongo-express-service 8081:8081
```

## 🚀 Production Considerations

### For Production Deployment:

1. **Persistent Storage**: Add PersistentVolumes for MongoDB data
2. **Secrets Management**: Use external secret management (Vault, etc.)
3. **Ingress**: Replace NodePort with Ingress for HTTPS access
4. **Monitoring**: Add Prometheus/Grafana monitoring
5. **Backup**: Implement automated backup solutions
6. **Network Policies**: Add network policies for security
7. **Resource Requests**: Tune resource requests based on workload

### Example Persistent Volume Addition:

```yaml
volumeMounts:
  - name: mongo-storage
    mountPath: /data/db
volumes:
  - name: mongo-storage
    persistentVolumeClaim:
      claimName: mongo-pvc
```

## 🐛 Common Issues

### Authentication Failed

- Verify secret values are correctly base64 encoded
- Check if MongoDB deployment is using correct secret keys

### Pod Not Starting

- Check resource limits vs available cluster resources
- Verify image pull policies and image availability

### Cannot Access Mongo Express

- Confirm NodePort service is running: `kubectl get svc`
- Check if port 30081 is accessible on your cluster nodes
- Verify Minikube IP: `minikube ip`

## 📝 Version History

- **v1.0.0**: Initial release with MongoDB and Mongo Express deployment

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Commit changes: `git commit -am 'Add feature'`
4. Push to branch: `git push origin feature-name`
5. Submit a Pull Request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👨‍💻 Author

**Sirwan** - [NaserRaoofi](https://github.com/NaserRaoofi)

---

**⭐ If this project helped you, please give it a star!**
