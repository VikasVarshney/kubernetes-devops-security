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
  }
}