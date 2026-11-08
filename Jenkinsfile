def dockerImage

pipeline {

  environment {
    dockerimagename = "malnakib01/react-app"
  }

  agent any

  stages {

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = '13b98cb8-2a1d-488d-864e-f9ecb788f07e'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG')]) {
          sh 'kubectl apply -f deployment.yaml -f service.yaml'
          sh 'kubectl rollout restart deployment/deployment'
          sh 'kubectl rollout status deployment/deployment'
        }
      }
    }

  }

}
