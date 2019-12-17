pipeline {

  environment {
    registry = 'anuraagrijal/hello-world-jenkins-kube'
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/anuraagrijal3138/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }

    stage('Deploy App') {
      steps {
        script {
          sh 'sed s/image-name/"anuraagrijal/hello-world-jenkins-kube" + ":$BUILD_NUMBER"/g myweb.yaml'
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }

  }

}
