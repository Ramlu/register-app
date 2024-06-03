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
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ramlu/register-app'
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
        stage('Sonarqube Analysis') {
            steps {
                script {
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        // some block
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage('Quality Gates') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }
        }
    }
}
