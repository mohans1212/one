pipeline {
  agent any
  environment {
    DOCKERHUB = "mohancloud12/one"
    REG_CRED_ID = "dockerpass"       // Jenkins credentialsId (username/password)
    CONTAINER_NAME = "tomcat_service"
  }

  stages {
    stage('Checkout') { steps { git branch: 'master', url: 'https://github.com/mohans1212/one' } }

    stage('Build') { steps { sh 'mvn clean package' } }

    stage('Build Image') {
      steps {
        script {
          IMAGE_TAG = "${DOCKERHUB}:${BUILD_NUMBER}"
          sh "docker build -t ${IMAGE_TAG} ."
          env.IMAGE_TAG = IMAGE_TAG
        }
      }
    }

    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: REG_CRED_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push ${IMAGE_TAG}
            docker image rm ${IMAGE_TAG} || true
            docker logout
          '''
        }
      }
    }

    stage('Deploy Local') {
      steps {
        sh """
          docker stop ${CONTAINER_NAME} || true
          docker rm ${CONTAINER_NAME} || true
          docker run -d --name ${CONTAINER_NAME} -p 8082:8080 ${IMAGE_TAG}
        """
      }
    }

    stage('Health Check') {
      steps {
        sh '''
          sleep 15
          if ! curl -f http://localhost:8082; then
            echo "Health check failed - showing logs"
            docker logs ${CONTAINER_NAME} || true
            exit 1
          fi
          echo "App healthy"
        '''
      }
    }
  }
}
