pipeline {
    agent {
        label 'Jenkins-Agent'
    }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout From Git') {
            steps {
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/Ramlu/register-app'
            }
        }
        stage('Build Clean') { 
        steps { 
                sh 'mvn clean package' 
            } 
        } 
    }
}
