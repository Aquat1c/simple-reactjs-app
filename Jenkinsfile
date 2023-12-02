pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'node:14'  // Use the Node.js 14 Docker image
        DOCKERHUB_CREDENTIALS = 'dockerhub_credentials'
        REACT_APP_NAME = 'simple-reactjs-app'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Install dependencies and build the React app
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

     stage('Push to DockerHub') {
            steps {
                script {
                    // Push Docker image to DockerHub
                    sh "docker login -u ${registry} -p ${dockerhubToken}"
                    sh "docker push ${dockerImage}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Rebuild and redeploy Docker containers
                    sh 'docker-compose down'
                    sh 'docker-compose up -d --build'
                }
        }

    post {
        always {
            // Cleanup workspace
            cleanWs()
        }
    }
}
