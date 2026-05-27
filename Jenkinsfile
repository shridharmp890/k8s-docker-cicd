pipeline {

    agent any

    environment {
        DOCKER_IMAGE = "shridhar8899/myapp"
    }

    stages {

        stage('Clone Code') {
            steps {
                git ''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }

        stage('Docker Login') {

            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'shridhar8899',
                    passwordVariable: 'Shri@8899'
                )]) {

                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Image') {
            steps {

                bat 'docker push %DOCKER_IMAGE%'
            }
        }

        stage('Deploy Kubernetes') {
            steps {

                bat 'kubectl apply -f deployment.yaml'

                bat 'kubectl apply -f service.yaml'
            }
        }
    }

    post {

        success {
            echo 'Deployment Successful!'
        }

        failure {

            echo 'Deployment failed. Rolling back...'

            bat 'docker stop mycontainer || exit 0'

            bat 'docker rm mycontainer || exit 0'
        }
    }
}
