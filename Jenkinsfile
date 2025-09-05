pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker'  
        DOCKERHUB_REPO = 'prathapxsz/turium'
        IMAGE_TAG = 'n8n'  // Use DinD sidecar
    }

    stages {

        stage('Check Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -lah'
            }
        }
        stage('Check Versions') {
            steps {
                sh 'pnpm install --frozen-lockfile'
                sh 'pnpm build:docker'
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKERHUB_CREDENTIALS) {
                        echo "Logged in to Docker Hub"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                   docker tag n8nio/n8n:n8n ${DOCKERHUB_REPO}:${IMAGE_TAG}
                   docker push ${DOCKERHUB_REPO}:${IMAGE_TAG}
                """
            }
        }


    }
}
