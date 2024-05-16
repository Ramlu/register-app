pipeline {
    agent {
        label 'Jenkins-Agent'
    }
    tools {
        jdk 'java17'
        maven 'maven3'
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
        stage("Trivy Scan") { 
           		steps { 
               			script { 
            			sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image naveen9700/register-app-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table') 
               			} 
          		 } 
       	} 
       	stage ('Cleanup Artifacts') { 
           		steps { 
               			script { 
                    			sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}" 
                    			sh "docker rmi ${IMAGE_NAME}:latest" 
               			} 
          		} 
       	} 
    }
}
