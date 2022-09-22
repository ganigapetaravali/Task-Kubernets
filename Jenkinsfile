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
//     stage('Email-Notification') {
//       steps {
//          emailext mimeType: 'text/html',               
//          subject: "[Jenkins]${currentBuild.fullDisplayName}",               
//          to: "nunakana.satish@testingxperts.com",             
//           body: """Please go to console output of ${BUILD_URL}input to approve or Reject"""    
//       input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
//          sh 'docker build -t flask:8.0 .'
//              }
//           }
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
  }
//      stage('slack notification') {
//          slackSend channel: "#kubernetes-task", color: "good", message: "Message from Jenkins Pipeline"
//             }
    }
  stage('selinium-test') {
               sh 'python app.py'
    steps {
        if (getContext(hudson.FilePath)) 
           }
       }
