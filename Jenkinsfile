pipeline {
        agent { label 'jenkins-agent' }

        tools {
                jdk 'Java17'
                maven 'Maven3'
        }

        stages {
                stage('CleanWS') {
                        steps {
                              cleanWs()  
                        }
                }
                stage('Checkout') {
                        steps {
                                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ramlu/register-app.git'
                        }
                }
                stage('Maven Clean Install') {
                        steps {
                                sh 'mvn clean install'
                        }
                }
                stage('Package'){
                        steps {
                                sh 'mvn package'
                        }
                }
        }
}
