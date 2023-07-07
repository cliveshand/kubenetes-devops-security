
pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }

    stage('Docker Build and Push cliveshand Repo') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
         // sh 'docker build -t cliveshand/numeric-app:""$GIT_COMMIT"" .'
         // sh 'docker push cliveshand/numeric-app:""$GIT_COMMIT""'


         sh 'docker build -t cliveshand/numeric-app:latest .'
         sh 'docker push cliveshand/numeric-app:latest'

        }
      }
    }
  }
}
