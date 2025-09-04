pipeline {
    agent any

    environment {
        IMAGE_NAME = "n8n"
        REGISTRY = "prathapxsz/turium"
        TAG = "${env.BUILD_NUMBER}"
        DOCKERFILE_PATH = "n8n/docker/images/n8n/Dockerfile"
        BUILD_CONTEXT = "n8n/docker/images/n8n"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}", "-f ${DOCKERFILE_PATH} ${BUILD_CONTEXT}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-registry-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh "echo $DOCKER_PASS | docker login $REGISTRY -u $DOCKER_USER --password-stdin"
                        sh "docker tag ${IMAGE_NAME}:${TAG} $REGISTRY/${IMAGE_NAME}:${TAG}"
                        sh "docker push $REGISTRY/${IMAGE_NAME}:${TAG}"
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}