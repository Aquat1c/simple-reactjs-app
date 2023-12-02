pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'node:14'  // Use the Node.js 14 Docker image
        DOCKERHUB_CREDENTIALS = 'dockerhub_credentials'
        REACT_APP_NAME = 'simple-reactjs-app'
        REGISTRY = 'your_registry_username'  // Replace with your Docker Hub username or registry URL
        DOCKERHUB_TOKEN = 'your_dockerhub_token'  // Replace with your Docker Hub token
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
            // Push Docker image to DockerHub using withDockerRegistry
            withDockerRegistry(credentialsId: DOCKERHUB_CREDENTIALS, url: 'https://registry.hub.docker.com') {
                sh "docker login -u ${registry} -p ${dockerhubToken}"
                sh "docker push ${dockerImage}"
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
