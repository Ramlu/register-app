pipeline {
	agent { label 'Jenkins-Agent'}
	tools {
		jdk 'Jdk17'
		maven 'Maven3'
	}
	stages {
		stage('Clean WS') {
			steps {
				cleanWs()
			}
		}
		stage('Checkout SCM') {
			steps {
				git branch: 'main', url: 'https://github.com/Ramlu/register-app.git'
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
	}
}
