pipeline {
 agent {
  label 'Jenkins-Agent'
 }
 stages {
  stage('Clean WS') {
   steps {
    cleanWs()
   }
  }
  stage('Checkout Git') {
   steps {
    git branch: 'main', credentialsId: 'Github', url: 'https://github.com/Ramlu/register-app'
   }
  }
  stage('Build and Clean') {
   steps {
    sh 'mvn clean package'
   }
  }
  stage('Test Application') {
   steps {
    sh 'mvn test'
   }
  }
 }
}
