pipeline {
    agent any

    environment {
        registry = 'aquatic'
        imageName = 'simple-reactjs-app'
        dockerImage = "${registry}/${imageName}"
        dockerhubToken = 'dckr_pat_ZGWUk_vd34gCFNEiCNZrsBxPOzA'
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

                    // Build Docker image with the specified tag
                    sh "docker build -t ${dockerImage} ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    // Push Docker image to DockerHub using credentials
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
        }
    }

    post {
        always {
            // Cleanup workspace
            cleanWs()
        }
    }
}
