pipeline {
	agent { label "Jenkins-Agent"}
	tools {
		jdk "Java17"
		maven "Maven3"
	}

	stages {
		stage('Clean Workspace') {
			steps {
				cleanOs()
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
	}
}
