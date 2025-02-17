pipeline {

  environment {
    dockerimagename = "fabiofedele69/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Git Checkout Source') {
            steps {
                script {
                    git branch: 'main',
                        credentialsId: 'Github-credentials',
                        url: 'https://github.com/fabiofedele69/jenkins-kubernetes-deployment.git'
                }
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
               registryCredential = 'dockerhub-credentials'
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
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
