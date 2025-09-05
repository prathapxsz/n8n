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


    }
}
