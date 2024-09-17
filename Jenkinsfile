pipeline {

  environment {
    KUBECONFIG = credentials('kubeconfig-credentials-id')
    dockerimagename = "pujayani/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/dpuja123/node-apptest.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'PUJAREPO1'
      }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {

          sh 'kubectl apply -f deploymentservice.yml'

        }
      }
    }

  }

  post {
    always {
      // Optional cleanup steps
      sh 'docker system prune -f'
    }
  }

}
