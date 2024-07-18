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
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/Ramlu/register-app.git'
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
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage('Quality Gates') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
