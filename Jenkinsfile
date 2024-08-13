pipeline {
    agent any
    
    environment {
        // Set Docker Hub credentials
        DOCKER_CREDENTIALS_ID = 'docker-hub'
        DOCKER_IMAGE_NAME = 'kishanchauhan55/test-cicd-timstamp'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${env.DOCKER_IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${env.DOCKER_CREDENTIALS_ID}") {
                        // Push the Docker image
                        docker.image("${env.DOCKER_IMAGE_NAME}:${env.BUILD_ID}").push('latest')
                        docker.image("${env.DOCKER_IMAGE_NAME}:${env.BUILD_ID}").push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Clean up Docker images
            sh 'docker system prune -af'
        }
    }
}

