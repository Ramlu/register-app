pipeline {

    agent {
        label "jenkins-agent"
    }

    tools {
        jdk 'jdk21'
        maven 'maven3'
    }

    stages {
        stage('CleanWS') {
            steps {
                cleanWs()
            }
        }
        stage('clone') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ramlu/register-app.git'
            }
        }
        stage('Maven clean') {
            steps {
                sh 'maven clean install'
            }
        }
        stage('Maven Test') {
            steps {
                sh 'maven test'
            }
        }
    }
}
