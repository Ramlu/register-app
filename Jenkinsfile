pipeline {
	agent { label "Jenkins-Agent"}
	tools {
		jdk "Java17"
		maven "Maven3"
	}
	// environment {
	// 	APP_NAME = "register-app-pipeline"
	// 	RELEASE = "1.0.0"
	// 	DOCKER_USER = "naveen9700"

	// 	DOCKER_CREDENTIALS = credentials('dockerhub') // Credential ID for Docker Hub credentials
	// 	DOCKER_IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
	// }
	environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "naveen9700"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
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
		        stage("Build & Push Docker Image") {
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
		// stage('Push to Docker Hub') {
  //           		steps {
  //               		// Push the Docker image to Docker Hub
  //               		script {
  //                   			docker.withRegistry('', DOCKER_CREDENTIALS) {
  //                       		docker.image(DOCKER_IMAGE_NAME).push('latest')
  //                   			}
  //               		}
  //        	   	}
		// }
		}
}
