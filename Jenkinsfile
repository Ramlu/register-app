pipeline {
agent { label "Jenkins-Agent"}
tools {
jdk "Java17"
maven "Maven3"
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
sh 'mvn -e clean install'
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
