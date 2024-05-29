pipeline {
    agent { label 'jenkins-agent' }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git Checkout') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/Ramlu/register-app.git'
            }
        }
        stage('Clean Package') {
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
