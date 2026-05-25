pipeline {
    agent any
    environment {
        IMAGE_NAME   = "shridharmp890/devops-app"
        DOCKER_CREDS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/shridharmp890/k8s-docker-cicd'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh "echo ${DOCKER_CREDS_PSW} | docker login -u ${DOCKER_CREDS_USR} --password-stdin"
                sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                sh "docker push ${IMAGE_NAME}:latest"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f k8s/deployment.yaml"
                sh "kubectl apply -f k8s/service.yaml"
                sh "kubectl rollout status deployment/devops-app"
            }
        }
        stage('Verify Deployment') {
            steps {
                sh "kubectl get pods"
                sh "kubectl get services"
                sh "kubectl get hpa"
            }
        }
    }
    post {
        success {
            echo 'App deployed successfully to Kubernetes!'
        }
        failure {
            echo 'Deployment failed. Rolling back...'
            sh 'kubectl rollout undo deployment/devops-app'
        }
    }
}
