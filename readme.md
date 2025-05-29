# 3T WebApp Frontend Helm Chart

A production-ready Helm chart for deploying the React frontend component of the 3T WebApp on Kubernetes clusters with automated scaling, CI/CD integration, and comprehensive monitoring capabilities.

## 🚀 Overview

This repository provides a complete Kubernetes deployment solution for React frontend applications using Helm charts. It includes automated scaling, service configuration, and CI/CD pipeline integration to ensure reliable and scalable deployments.

### Key Features

- **🎯 Production-Ready Deployment**: Kubernetes best practices implementation
- **📈 Auto-Scaling**: Horizontal Pod Autoscaler (HPA) for dynamic scaling
- **🔄 CI/CD Integration**: Jenkins pipelines with SonarQube quality gates
- **🛠️ Flexible Configuration**: Customizable Helm values for different environments
- **📊 Monitoring Ready**: Built-in metrics and health checks

## 📁 Repository Structure

```
3t-webapp-frontend-helm/
├── frontend-helm-custom/          # Main Helm chart directory
│   ├── charts/                    # Dependency charts
│   ├── templates/                 # Kubernetes manifest templates
│   │   ├── deployment.yaml        # Application deployment
│   │   ├── service.yaml          # Service configuration
│   │   └── hpa-frontend.yaml     # Horizontal Pod Autoscaler
│   ├── values.yaml               # Default configuration values
│   └── Chart.yaml                # Helm chart metadata
├── frontend/                     # React application source code
├── Jenkinsfile                   # Standard CI/CD pipeline
├── Jenkinsfile-sonar            # CI/CD with SonarQube integration
└── .gitignore                   # Git ignore configuration
```

## 📋 Prerequisites

Before deploying, ensure you have:

- **Kubernetes Cluster**: v1.20+ running and accessible
- **Helm**: v3.x installed on your local machine
- **kubectl**: Configured and connected to your cluster
- **Docker Registry Access**: For pulling/pushing container images
- **Jenkins** (optional): For CI/CD pipeline automation
- **SonarQube** (optional): For code quality analysis

## 🛠️ Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/AnonyIIMessiah/3t-webapp-frontend-helm.git
cd 3t-webapp-frontend-helm
```

### 2. Navigate to Helm Chart

```bash
cd frontend-helm-custom
```

### 3. Review and Customize Configuration

Edit `values.yaml` to match your environment:

```yaml
# Example configuration
image:
  repository: your-registry/3t-webapp-frontend
  tag: "latest"
  pullPolicy: IfNotPresent

replicaCount: 2

service:
  type: ClusterIP
  port: 80
  targetPort: 3000

resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"

autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 80
```

### 4. Deploy the Application

```bash
# Create namespace (if needed)
kubectl create namespace 3t-webapp

# Install the Helm chart
helm install 3t-frontend . -n 3t-webapp
```

### 5. Verify Deployment

```bash
# Check all resources
kubectl get all -n 3t-webapp

# Check pod status
kubectl get pods -n 3t-webapp

# Check service endpoints
kubectl get svc -n 3t-webapp
```

## 🔧 Configuration Options

### Core Application Settings

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Container image repository | `3t-webapp-frontend` |
| `image.tag` | Image tag | `latest` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `replicaCount` | Number of replicas | `1` |

### Service Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.type` | Kubernetes service type | `ClusterIP` |
| `service.port` | Service port | `80` |
| `service.targetPort` | Container port | `3000` |

### Resource Management

| Parameter | Description | Default |
|-----------|-------------|---------|
| `resources.requests.memory` | Memory request | `64Mi` |
| `resources.requests.cpu` | CPU request | `250m` |
| `resources.limits.memory` | Memory limit | `128Mi` |
| `resources.limits.cpu` | CPU limit | `500m` |

### Auto-Scaling Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `autoscaling.enabled` | Enable HPA | `false` |
| `autoscaling.minReplicas` | Minimum replicas | `1` |
| `autoscaling.maxReplicas` | Maximum replicas | `10` |
| `autoscaling.targetCPUUtilizationPercentage` | CPU threshold | `80` |


### Health Checks

```bash
# Check pod health
kubectl describe pods -n 3t-webapp

# View application logs
kubectl logs -f deployment/3t-frontend -n 3t-webapp

# Test service connectivity
kubectl port-forward svc/3t-frontend 8080:80 -n 3t-webapp
# Access: http://localhost:8080
```

## 🔄 CI/CD Pipelines

### Standard Pipeline (`Jenkinsfile`)

The standard pipeline includes:

1. **Source Checkout**: Clone repository
2. **Build Stage**: Create Docker image
3. **Test Stage**: Run unit and integration tests
4. **Deploy Stage**: Deploy using Helm
5. **Verification**: Validate deployment

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') { ... }
        stage('Build') { ... }
        stage('Test') { ... }
        stage('Deploy') { ... }
        stage('Verify') { ... }
    }
}
```

### SonarQube Pipeline (`Jenkinsfile-sonar`)

Enhanced pipeline with code quality analysis:

1. **All Standard Stages**
2. **Code Analysis**: SonarQube scanning
3. **Quality Gates**: Enforce quality standards
4. **Security Scanning**: Vulnerability detection
5. **Quality Reports**: Generate detailed reports

## 🚀 Deployment Strategies

### Development Environment

```bash
helm install 3t-frontend . \
  --namespace dev \
  --set image.tag=dev-latest \
  --set replicaCount=1 \
  --set autoscaling.enabled=false
```

### Staging Environment

```bash
helm install 3t-frontend . \
  --namespace staging \
  --set image.tag=staging-v1.0.0 \
  --set replicaCount=2 \
  --set autoscaling.enabled=true \
  --set autoscaling.maxReplicas=5
```

### Production Environment

```bash
helm install 3t-frontend . \
  --namespace production \
  --set image.tag=v1.0.0 \
  --set replicaCount=3 \
  --set autoscaling.enabled=true \
  --set autoscaling.maxReplicas=20 \
  --set resources.requests.memory=128Mi \
  --set resources.limits.memory=256Mi
```

## 🔄 Updates and Maintenance

### Upgrading the Application

```bash
# Update image tag
helm upgrade 3t-frontend . \
  --set image.tag=v1.1.0 \
  -n 3t-webapp
```

### Rolling Back

```bash
# Rollback to previous version
helm rollback 3t-frontend 1 -n 3t-webapp

# View rollback history
helm history 3t-frontend -n 3t-webapp
```

### Scaling Operations

```bash
# Manual scaling
kubectl scale deployment 3t-frontend --replicas=5 -n 3t-webapp

# Update HPA settings
helm upgrade 3t-frontend . \
  --set autoscaling.maxReplicas=15 \
  --set autoscaling.targetCPUUtilizationPercentage=70 \
  -n 3t-webapp
```

## 🐛 Troubleshooting

### Common Issues

1. **Pod Not Starting**
   ```bash
   kubectl describe pod <pod-name> -n 3t-webapp
   kubectl logs <pod-name> -n 3t-webapp
   ```

2. **Service Not Accessible**
   ```bash
   kubectl get endpoints -n 3t-webapp
   kubectl describe svc 3t-frontend -n 3t-webapp
   ```

3. **HPA Not Scaling**
   ```bash
   kubectl describe hpa 3t-frontend -n 3t-webapp
   kubectl top pods -n 3t-webapp
   ```

### Debug Commands

```bash
# Check Helm deployment status
helm status 3t-frontend -n 3t-webapp

# Validate Helm templates
helm template 3t-frontend . --debug

# Check resource usage
kubectl top pods -n 3t-webapp
kubectl top nodes
```

## 📊 Monitoring and Observability

### Metrics Collection

The deployment includes:
- **Resource Metrics**: CPU, memory usage
- **Application Metrics**: Request rates, response times
- **Health Checks**: Liveness and readiness probes

### Prometheus Integration

```yaml
# Add to values.yaml for Prometheus monitoring
monitoring:
  enabled: true
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/port: "3000"
    prometheus.io/path: "/metrics"
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test thoroughly
5. Commit your changes (`git commit -m 'Add some amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

### Development Guidelines

- Follow Kubernetes best practices
- Update documentation for any configuration changes
- Test on multiple environments before submitting
- Ensure CI/CD pipelines pass successfully

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

For issues and questions:

1. Check the [Issues](https://github.com/AnonyIIMessiah/3t-webapp-frontend-helm/issues) page
2. Review troubleshooting section above
3. Create a detailed issue with:
   - Environment details
   - Steps to reproduce
   - Expected vs actual behavior
   - Relevant logs and configurations

## 🙏 Acknowledgments

- [Helm Community](https://helm.sh/) for the excellent package manager
- [Kubernetes](https://kubernetes.io/) for the container orchestration platform
- [React](https://reactjs.org/) for the frontend framework
- [Jenkins](https://www.jenkins.io/) for CI/CD automation
- [SonarQube](https://www.sonarqube.org/) for code quality analysis

---

**Happy Deploying! 🚀✨**
