pipeline {

  environment {
    dockerimagename = "sandhyadockerac/react-app"
    dockerImage = ""
  }

   agent { 
     docker{
        label "docker-slave"
        args '-v /var/run/docker.sock:/var/run/docker.sock'
     }
     }
   tools{
        dockerTool 'docker'
    }

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/asmimore/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhubloginsan'
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
        script {
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
