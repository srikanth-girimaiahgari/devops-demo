pipeline {

  agent {
    kubernetes {
      label 'jenkins-agent'
    }
  }

  environment {
    IMAGE_NAME = "gsrikanth14/devops-demo"
  }

  stages {

    stage('Clone Repo') {
      steps {
        git 'https://github.com/srikanth-girimaiahgari/devops-demo.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME:latest .'
      }
    }

    stage('Login to DockerHub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
        }
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push $IMAGE_NAME:latest'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
        sh 'kubectl get pods'
      }
    }

  }

}