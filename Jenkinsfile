pipeline {

  environment {
    dockerimagename = "shubham1911/react-app"
    dockerImage = ""
  }
  agent any
 //  agent { 
  //      label "docker-slave"
   //  }
  //agent { 
   //  docker{
   //    label "docker-slave"
    //    image 'my-jenkins-slave:latest'
     //   args '-v /var/run/docker.sock:/var/run/docker.sock'
    // }
     //}
//tools{
  //     dockerTool 'docker'
   //}

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/asmimore/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          //tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
  stage('Apply Kubernetes files') {
    steps {
      script {
    withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://192.168.10.184:6443']) {
      sh 'kubectl apply -f my-kubernetes-directory'
    }
    }
    }
  }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }

  }

}
