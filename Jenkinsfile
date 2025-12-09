pipeline{
    agent any
    environment{
        IMAGE_NAME = "${BUILD_TAG}:${BUILD_ID}"
        CONTAINER_NAME = "tomcat_service"
    }

    stages{
        stage('Checkout'){
            steps{
                git branch:'master',url:'https://github.com/mohans1212/one'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Image Creation'){
            steps{
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Build Deploy'){
            steps{
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker run -d --name ${CONTAINER_NAME} -p 8080:8080 ${IMAGE_NAME}"
            }
        }
    }
    
}

