pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }
    environment {
        DOCKER_PASS = credentials('DOCKER_PASS')
        IMAGE_TAG = "${BUILD_NUMBER}"
        REACT_APP_API_URL = "${REACT_APP_API_URL}"
    }
    stages {
        stage('Preparation') {
            steps {
                script {
                    // Clean workspace before starting
                    sh 'apt-get update && apt-get install -y docker.io'
                }
            }
        }

        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                dir('frontend') {
                    script {
                        sh 'echo "REACT_APP_API_URL=${REACT_APP_API_URL}" > .env'
                        sh 'docker build -t demoniiexe/microservice-frontend:"${IMAGE_TAG}" .'
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    
                        // Login to Docker Hub
                        sh 'docker login -u demoniiexe -p $DOCKER_PASS'
                        sh 'docker push demoniiexe/microservice-frontend:"${IMAGE_TAG}"'
                    
                }
            }
        }
        stage('EKS-setup') {
            steps {
                script {
                    sh 'aws eks update-kubeconfig --name task-eks --region ap-south-1'
                }
            }
        }
        stage('Deploy') {
            
            steps {
                    script {
                        def releaseName = "frontend"
                        def namespace = "default"

                        def isInstalled = sh(
                            script: "helm list -n ${namespace} -q | grep -w ${releaseName} || true",
                            returnStdout: true
                        ).trim()

                        if (isInstalled) {
                            echo "Helm release '${releaseName}' is already installed."
                            sh 'helm upgrade frontend frontend-helm-custom --set deployment.image.tag="${IMAGE_TAG}'
                        } else {
                            echo "Helm release '${releaseName}' is not installed. Installing now..."
                            sh 'helm install frontend frontend-helm-custom --set deployment.image.tag="${IMAGE_TAG}"'
                        }
                    }
            }   
        }
    }
}
