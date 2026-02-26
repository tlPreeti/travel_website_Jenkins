pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'preetitl'
        DOCKERHUB_REPO = 'travel-site'
        DOCKER_IMAGE = 'webapp'
        VERSION = "${env.BUILD_ID}"
        CONTAINER_NAME = 'app'
        CONTAINER_PORT = '8003'
        REQUEST_PORT = '80'
    }

    stages {

        stage("docker version") {
            steps {
                sh "sudo docker --version"
            }
        }

        stage("to check docker images") {
            steps {
                sh "sudo docker images"
            }
        }

        stage("build docker image") {
            steps {
                sh "sudo docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage("build docker tag") {
            steps {
                sh """
                sudo docker tag ${DOCKER_IMAGE} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:${VERSION}
                sudo docker tag ${DOCKER_IMAGE} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest
                """
            }
        }

        stage("docker images") {
            steps {
                sh "sudo docker images"
            }
        }

        stage("login and push to docker hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh """
                    echo \$DOCKER_PASS | sudo docker login -u \$DOCKER_USER --password-stdin
                    sudo docker push ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:${VERSION}
                    sudo docker push ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest
                    """
                }
            }
        }

        stage("Run container") {
            steps {
                sh """
                sudo docker stop ${CONTAINER_NAME} || true
                sudo docker rm ${CONTAINER_NAME} || true
                sudo docker run -d --name ${CONTAINER_NAME} -p ${CONTAINER_PORT}:${REQUEST_PORT} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest
                """
            }

            post {
                always {
                    echo "Docker container from server"
                }
                success {
                    echo "Docker container deployed successfully"
                }
                failure {
                    echo "Docker container is not deployed"
                    sh "sudo docker rm -f ${CONTAINER_NAME} || true"
                }
            }
        }
    }
}