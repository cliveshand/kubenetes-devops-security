pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and JaCoCo') {
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

    stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
      post {
        always {
          pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    }

    // stage('SonarQube - SAST') {
    //   steps {
    //     withSonarQubeEnv('SonarQube') {
    //    // sh "docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest"
    //       sh "mvn clean verify sonar:sonar  -Dsonar.projectKey=numeric-application -Dsonar.projectName='numeric-application' -Dsonar.host.url=http://4.236.131.121:9000 -Dsonar.token=sqp_01cd09e2ff6125c01de5f5900b02a7c9b1b9d9da"
    //     }
    //     timeout(time: 2, unit: 'MINUTES') {
    //       script {
    //         waitForQualityGate abortPipeline: true
    //       }
    //     }
    //   }
    // }

    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t cliveshand/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push cliveshand/numeric-app:""$GIT_COMMIT""'
        }
      }
    }

    stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
          sh "sed -i 's#replace#siddharth67/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
        }
      }
    }

  }

}
