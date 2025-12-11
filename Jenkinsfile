pipeline {
    agent any

    environment {
        DOCKERHUB = "mohancloud12/one"
        REGISTRY_CREDENTIAL = "dockerpass"    // Jenkins credentialsId
        CONTAINER_NAME = "tomcat_service"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/mohans1212/one'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // Local variable (no Jenkins warning)
                    def IMAGE_TAG = "${DOCKERHUB}:${BUILD_NUMBER}"

                    // Build docker image
                    docker.build(IMAGE_TAG)

                    // Make available for next stages
                    env.IMAGE_TAG = IMAGE_TAG

                    // Show build number in Jenkins UI
                    currentBuild.displayName = "#${BUILD_NUMBER}"
                }
            }
        }

        stage('Deploy Local (8082)') {
            steps {
                sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p 8082:8080 ${IMAGE_TAG}
                """
            }
        }

        // Optional health-check stage
        // stage('Health Check') {
        //     steps {
        //         sh '''
        //             sleep 10
        //             curl -f http://localhost:8082 || exit 1
        //             echo "Container healthy!"
        //         '''
        //     }
        // }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIAL) {
                        docker.image(env.IMAGE_TAG).push()
                        docker.image(env.IMAGE_TAG).push('latest')
                    }
                }
            }
        }
    }
}
