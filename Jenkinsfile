pipeline {
	agent { label 'Jenkins-Agent'}

	tools {
		jdk 'Java17'
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
	}
}
