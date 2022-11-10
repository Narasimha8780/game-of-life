pipeline {
  environment {
    registry = "narasimha8780/games"
    registryCredential = 'dockerhub credentials'
    dockerImage = ''
  }    
  agent any
  tools {maven "maven363" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Narasimha8780/game-of-life.git'
      }
    }
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
         }
       }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
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
        sh "docker run -d --name dockerpipeline -p 8088:8080 narasimha8780/games:$BUILD_NUMBER"
        
      }
    }     
      }
   }
