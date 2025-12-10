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
    }
}