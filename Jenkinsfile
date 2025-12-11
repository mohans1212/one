pipeline {
  agent any
  environment {
    DOCKERHUB = "mohancloud12/one"
    REGISTRY_CREDENTIAL = 'dockerpass'      // Jenkins credentialsId (username/password)
    CONTAINER_NAME = "tomcat_service"
  }

  stages {
    stage('Checkout') {
      steps { git branch: 'master', url: 'https://github.com/mohans1212/one' }
    }

    stage('Build') {
      steps { sh 'mvn clean package' }
    }

    stage('Build Image') {
      steps {
        script {
          IMAGE_TAG = "${DOCKERHUB}:${BUILD_NUMBER}"
          def img = docker.build(IMAGE_TAG)   // builds and returns image object
          env.IMAGE_TAG = IMAGE_TAG
          currentBuild.displayName = "#${BUILD_NUMBER}"
        }
      }
    }

    stage('Deploy (local)') {
      steps {
        sh """
          docker stop ${CONTAINER_NAME} || true
          docker rm ${CONTAINER_NAME} || true
          docker run -d --name ${CONTAINER_NAME} -p 8082:8080 ${IMAGE_TAG}
        """
      }
    }

    // optional health-check stage here

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIAL) {
            def img = docker.image(env.IMAGE_TAG)
            img.push()
            img.push('latest')
          }
        }
      }
    }
  }
}