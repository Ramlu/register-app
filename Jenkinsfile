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

}
}
