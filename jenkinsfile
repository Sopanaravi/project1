pipeline {
    agent any
    environment {
        // Set environment variables for Docker
        IMAGE_NAME = 'ubuntu'
        DOCKER_REGISTRY = 'dockerhub_username'
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the repository
                git branch: 'main', url: 'https://github.com/Sopanaravi/project1/new/main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ."
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using Jenkins credentials
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_REGISTRY --password-stdin"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    sh "docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
        stage('Cleanup') {
            steps {
                script {
                    // Remove Docker image locally after pushing
                    sh "docker rmi ${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
    post {
        success {
            echo "Docker Image built and pushed successfully!"
        }
        failure {
            echo "Build failed!"
        }
    }
}
