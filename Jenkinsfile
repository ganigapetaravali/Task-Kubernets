pipeline{
  agent any
  environment {
        imageName = "docker-image"
        registryCredentials = "nexus"
        registry = "18.212.25.74:8001/repository/k8s-task/"
        dockerImage = ''
    }
  stages{
    stage('checkout'){
      steps{
         checkout([$class: 'GitSCM', branches: [[name: '**']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/utsav1313/Task-Kubernets.git']]])
      }
    }
  }
     stage('Building image') {
      steps{
        script {
          sh 'docker build -t flask:7.0 .'
        }
      }
    }
    stage('Uploading to Nexus') {
     steps{  
         script {
             sh 'docker tag flask:3.0 18.212.25.74:8001/repository/k8s-task/flask:6.0'
             sh 'docker login -u admin -p ravali 18.212.25.74:8001/repository/k8s-task/' 
             sh 'docker push 18.212.25.74:8001/repository/k8s-task/flask:6.0'
             sh 'docker logout http://18.212.25.74:8001/repository/k8s-task/'
          }
        }
      }
    stage('Pre Prod..') {
     steps{  
         script {
             sh ' docker run -it -d -p 9090:9090 --name demo localhost:9091/docker-image'
        }
      }
    }
  }
