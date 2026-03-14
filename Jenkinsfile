pipeline {

  agent {
    kubernetes {
      inheritFrom 'jenkins-agent'
    }
  }

  environment {
    IMAGE_NAME = "gsrikanth14/devops-demo"
  }

  stages {

    stage('Clone Repo') {
      steps {
            git branch: 'main',
            url: 'https://github.com/srikanth-girimaiahgari/devops-demo.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        container('docker-image') {
        sh 'docker build -t $IMAGE_NAME:latest .'
        }
      }
    }

    stage('Login to DockerHub') {
      steps {
        container('docker-image') {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
        }
        }
      }
    }

    stage('Push Image') {
      steps {
        container('docker-image') {
        sh 'docker push $IMAGE_NAME:latest'
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        container('docker-image') {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
        sh 'kubectl get pods'
      }
      }
    }

  }

}