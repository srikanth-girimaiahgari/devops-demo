pipeline {

    agent any

    environment {
        IMAGE_NAME = "gsrikanth14/devops-demo:1.0"
    }

    stages {

        stage('Checkout') {
            steps {
                sh 'git clone "https://github.com/srikanth-girimaiahgari/devops-demo.git"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-demo:1.0 .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag devops-demo:1.0 $IMAGE_NAME'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }

    }

    post {
        always {
            cleanWs()
        }
    }
}