pipeline {
  environment {
    registry = "18.212.25.74:8001/repository/k8s-task/"
    registryCredential = 'nexus'
    dockerImage = ''
    SCANNER_HOME = tool 'sonarscanner'
    EMAIL_TO = 'ravali.ganigapeta@testingxperts.com'
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git branch: 'main', url: 'https://github.com/ganigapetaravali/Task-Kubernets.git'
      }
    }   
  stage('Building image') {
    steps{
      script {
        sh 'docker build -t flask:8.0 .'
        }
      }
    }
  stage('Deploy Image in to nexus registry') {
    steps{
      script {
       //sh 'curl "admin:ravali" -X PUT http://18.212.25.74:8001/repository/k8s-task/flask:8.0 '
        //flask:3.0.push("latest")
         sh 'docker tag flask:8.0 18.212.25.74:8001/repository/k8s-task/flask:8.0'
         //sh 'docker login -u ravali1505 -p Manoj@123@123'
         sh 'docker login -u admin -p ravali 18.212.25.74:8001/repository/k8s-task/' 
         sh 'docker push 18.212.25.74:8001/repository/k8s-task/flask:8.0'
         sh 'docker logout http://18.212.25.74:8001/repository/k8s-task/'
        }
      }
    }
    stage('Sonarqube') {
      environment {
     scannerHome = tool 'sonarscanner'
     }
     steps {
         withSonarQubeEnv('productionsonarqubescanner') {
         sh "${scannerHome}/bin/sonar-scanner"
           }
        }
    }
       stage ('Send Email') {
        echo "Mail Stage";

         mail to: "ravali.ganigapeta@testingxperts.com",
         cc: 'ravali.ganigapeta@testingxperts.com', charset: 'UTF-8', 
         from: 'ravali.ganigapeta@testingxperts.com', mimeType: 'text/html', replyTo: '', 
         bcc: '',
         subject: "jenkins test email: Project name -> ${env.JOB_NAME}",
         body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}";
       }
      }
   }
}
