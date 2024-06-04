pipeline {
    agent { label 'jenkins-agent' }

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
        stage('Checkout Git') {
            steps {
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/Ramlu/register-app'
            }
        }
        stage('Maven Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Maven Install') {
            steps {
                sh 'mvn install'
            }
        }
    }
}
