# 3T WebApp Frontend Helm Chart

![Helm](https://img.shields.io/badge/Helm-Chart-blue?logo=helm)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Deployed-blue?logo=kubernetes)

## 📦 Overview

This repository contains the Helm chart for deploying the frontend component of the 3T WebApp on a Kubernetes cluster. It facilitates streamlined deployment, scalability, and management of the frontend application using Kubernetes best practices.

## 🗂️ Repository Structure

```
3t-webapp-frontend-helm/
├── frontend-helm-custom/           # Helm chart for frontend deployment
│   ├── charts/                # Subcharts (if any)
│   ├── templates/             # Kubernetes manifest templates
│   │   ├── deployment.yaml    # Deployment configuration
│   │   ├── service.yaml       # Service configuration
│   │   └── hpa-frontend.yaml  # HPA configuration 
│   ├── values.yaml            # Default configuration values
│   └── Chart.yaml             # Helm chart metadata
├── frontend/                  # Frontend(React) and Dockerfile s
├── Jenkinsfile                # CI/CD pipeline configuration
├── Jenkinsfile-sonar          # CI/CD pipeline with SonarQube integration
├── test.sh                    # Shell script for testing deployments
└── .gitignore                 # Git ignore rules
```

## 🚀 Deployment Instructions

### Prerequisites

- A running Kubernetes cluster
- Helm installed on your local machine
- Docker registry access (if using private images)

### Steps

1. **Clone the Repository**

   ```bash
   git clone https://github.com/AnonyIIMessiah/3t-webapp-frontend-helm.git
   cd 3t-webapp-frontend-helm/frontend-helm-custom
   ```

2. **Customize Configuration**

   Edit the `values.yaml` file to set your desired configuration values:

3. **Deploy with Helm**

   ```bash
   helm install 3t-frontend . -n <namespace>
   ```

   Replace `<namespace>` with your target Kubernetes namespace.

4. **Verify Deployment**

   ```bash
   kubectl get all -n <namespace>
   ```

   Ensure all resources are up and running.

## 🧪 Testing

The `test.sh` script can be used to run the app.

```bash
chmod +x test.sh
./test.sh
```

Ensure you have the necessary permissions and that the script is configured correctly for your environment.

## ⚙️ CI/CD Integration

The repository includes two Jenkins pipeline configurations:

- **`Jenkinsfile`**: Defines the standard CI/CD pipeline for building, testing, and deploying the application.
- **`Jenkinsfile-sonar`**: Extends the standard pipeline with SonarQube integration for code quality analysis.

These pipelines automate the process of deploying the Helm chart to your Kubernetes cluster, ensuring consistent and reliable releases.

## 📄 License

This project is licensed under the MIT License. See the LICENSE file for details.

---

*For any issues or contributions, please open an issue or submit a pull request.*
