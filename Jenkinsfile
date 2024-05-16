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
        stage('Test Application') { 
        steps { 
            sh 'mvn test' 
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
        stage('Quality Gate') {
            steps { 
                script { 
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'      
                }	 
            }
        }
    }
}
