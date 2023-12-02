pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'node:14'  // Use the Node.js 14 Docker image
        DOCKERHUB_CREDENTIALS = 'dockerhub_credentials'
        REACT_APP_NAME = 'simple-reactjs-app'
        REGISTRY = 'sayaquatic'
        DOCKERHUB_TOKEN = credentials('dockerhub_credentials')
        DOCKER_IMAGE_NAME = "${REGISTRY}/${REACT_APP_NAME}"
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
                    // Push Docker image to DockerHub using credentials
                    withDockerRegistry([credentialsId: DOCKERHUB_CREDENTIALS, url: 'https://registry.hub.docker.com']) {
                        sh "docker login -u ${REGISTRY} --password-stdin ${DOCKERHUB_TOKEN}"
                        sh "docker push ${DOCKER_IMAGE_NAME}"
                    }
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
        }
    }

    post {
        always {
            // Cleanup workspace
            cleanWs()
        }
    }
}
