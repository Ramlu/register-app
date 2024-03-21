agent { label "Jenkins-Agent" }
	tools {
		jdk 'Java17'
		maven 'Maven3'
	}

	stages {
		stage('Clean Workspace') {
			steps {
				cleanWs()
			}
		}
		stage('Checkout SCM') {
			steps {
				git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ashfaque-9x/register-app'
			}
		}
		stage('Maven Package') {
			steps {
				sh 'mvn clean package'
			}
		}
		stage('Install') {
			steps {
				sh 'mvn install'
			}
		}
	}
}
