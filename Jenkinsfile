pipeline{
    agent any

    environment{
        DOCKERHUB_USERNAME = 'preetitl'
        DOCKERHUB_REPO = 'travel-site'
        DOCKER_IMAGE = 'webapp'
        VERSION = '$BUILD_ID'
    }

    stages{
        stage("docker version"){
            steps{
                sh "sudo docker --version"
            }
        }

        stage("to check docker images"){
            steps{
                sh "sudo docker images"
                }
        }
        stage("build docker image"){
            steps{
                sh "sudo docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage("build docker tag"){
            steps{
                sh """
                sudo docker tag ${DOCKER_IMAGE} ${DOCKERHUB_USERNAME}/${DOCKER_IMAGE}:${VERSION} 
                sudo docker tag ${DOCKER_IMAGE} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest
                """
            }
        }
    }
}