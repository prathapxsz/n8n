pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker'  
        DOCKERHUB_REPO = 'prathapxsz/turium'
        IMAGE_TAG = 'n8n'
        DOCKER_HOST = 'tcp://localhost:2375'   // Use DinD sidecar
    }

    stages {

        stage('Check Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -lah'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                export DOCKER_HOST=tcp://localhost:2375
                export DOCKER_TLS_VERIFY=
                export DOCKER_CERT_PATH=
                docker build -t $DOCKERHUB_REPO:$IMAGE_TAG -f docker/images/n8n/Dockerfile .
            '''
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
