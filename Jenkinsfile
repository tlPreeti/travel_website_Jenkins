pipeline{
    agent any

    environment{
        DOCKERHUB_USERNAME = 'preetitl'
        DOCKER_IMAGE = 'travel-site'
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
                sh "sudo docker build -t ${DOCKERHUB_USERNAME}/${DOCKER_IMAGE}:${VERSION}"
            }
        }
    }
}