## Docker Build and Push Stage
## replace  siddharth67 with your dockerhub username

pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    

    
  }
}