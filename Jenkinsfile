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
                sh "docker build -t ${IMAGE_NAME}"
            }
        }
    }
    
}

