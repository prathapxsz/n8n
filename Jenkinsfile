pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker'  
        DOCKERHUB_REPO = 'prathapxsz/turium'
        IMAGE_TAG = 'n8n'
        DOCKER_HOST = 'tcp://localhost:2375'   // Use DinD sidecar
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using Dockerfile in n8n/docker/images/n8n
                    dockerImage = docker.build(
                        "${env.DOCKERHUB_REPO}:${env.IMAGE_TAG}",
                        "-f /docker/images/n8n/Dockerfile ."
                    )
                }
            }
        }

        stage('Login to DockerHub and Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKERHUB_CREDENTIALS) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
