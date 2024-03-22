pipeline {
	agent { label 'Jenkins-Agent' }
	tools {
		jdk 'Java17'
		maven 'Maven3'
	}
	environment { 
           APP_NAME = "register-app-pipeline" 
            RELEASE = "1.0.0" 
            DOCKER_USER = "naveen9700" 
            DOCKER_PASS = 'dockerhub' 
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}" 
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}" 
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
		stage('Maven Pacakge') {
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
		stage('Quality Gate') {
			steps {
				script {
					waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'   
				}
			}
		}
		stage('Push the image') {
			steps {
				script {
					 docker.withRegistry('',DOCKER_PASS) { 
                        			docker_image = docker.build "${IMAGE_NAME}" 
                    			} 
                    			docker.withRegistry('',DOCKER_PASS) { 
                        			docker_image.push("${IMAGE_TAG}") 
                        			docker_image.push('latest') 
                    			}    
				}
			}
		}
	}
}
