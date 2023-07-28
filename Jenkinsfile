
pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

 

    // stage('Docker Build and Push') {
    //   steps {
    //     withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
    //       sh 'printenv'
    //  //   sh 'docker build -t siddharth67/numeric-app:""$GIT_COMMIT"" .'
    //  //   sh 'docker push siddharth67/numeric-app:""$GIT_COMMIT""'

    //       sh 'docker build -t cliveshand/numeric-app:""$GIT_COMMIT"" .'
    //       sh 'docker push cliveshand/numeric-app:""$GIT_COMMIT""'

    //     }
    //   }
    // }

    stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
         // sh "sed -i 's#replace#siddharth67/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
        }
      }
    }
    
  }

}