pipeline{
    agent any
    environment{
        IMAGE = "${BUILD_TAG}:${BUILD_ID}"
        CONTAINER_NAME = "tomcat"
    }
    stages{
        stage("Code"){
            steps{
                git branch:'master', url:'https://github.com/mohans1212/one'
            }
        }
        stage("Build"){
            steps{
            sh "mvn clean package"
            } 
        }
        stage("Image Build"){
            steps{
                sh "docker build -t ${IMAGE} ."
            }
        }
        stage("Deploy Image"){
            steps{
            sh "docker stop ${CONTAINER_NAME} || true"
            sh "docker rm ${CONTAINER_NAME} || true"
            sh "docker run -d -name ${CONTAINER_NAME} -p 8082:8080 $${IMAGE}"
            }
        }
    }
}