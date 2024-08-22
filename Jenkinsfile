pipeline {

agent { label 'jenkins-agent'}
tools {
        jdk 'Java17'
        maven 'Maven3'
}
stages {
        stage('Clean Workspace'){
                steps {
                        cleanWs()
                }
        }
        stage('Git checkout') {
                steps {
                        git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ramlu/register-app.git'
                }
        }
        stage('Maven Install') {
                steps {
                        sh 'mvn clean install'
                }
        }
        stage('Maven Package') {
                steps {
                        sh 'mvn package'
                }
        }
}
}
