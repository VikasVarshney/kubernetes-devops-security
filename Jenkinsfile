pipeline {
  agent any

  stages {
      stage('Building the Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later for project
            }
        }   
    
  stage('Doing Unit Tests - by JUnit and Jacoco') {
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

stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t vikasvarshney/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push vikasvarshney/numeric-app:""$GIT_COMMIT""'
        }
      }
    }
stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
          sh "sed -i 's#replace#vikasvarshney/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
        }
      }
    }


  }
}