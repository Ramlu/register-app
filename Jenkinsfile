pipeline {
	agent { label "Jenkins-Agent"}
	tools {
		jdk "Java17"
		maven "Maven3"
	}

	stages {
		stage('Clean Workspace') {
			steps {
				cleanWs()
			}
		}
		stage('Checkout SCM') {
			steps {
				git branch: 'main', url: 'https://github.com/Ramlu/register-app.git'
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
