#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the repository from your Git repository URL using the 'main' branch
                git branch: 'main', url: 'https://github.com/lakhwinder299/exam1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image using the Dockerfile from the repository
                script {
                    def dockerImage = docker.build("my_app:${env.BUILD_ID}", "-f Dockerfile .")
                    // Save the dockerImage variable as an environment variable for later use
                    env.IMAGE_NAME = "my_app:${env.BUILD_ID}"
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            environment {
                // Define your Docker Hub username and repository name
                DOCKER_HUB_USERNAME = 'lakhwinder299'
                DOCKER_HUB_REPOSITORY = 'my_app'
                // Define your Docker Hub password
                DOCKER_HUB_PASSWORD = 'dockerhub@123'
            }
            steps {
                // Tag the Docker image with Docker Hub repository name
                script {
                    sh "docker tag ${env.IMAGE_NAME} ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:${env.BUILD_ID}"
                    sh "docker tag ${env.IMAGE_NAME} ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:latest"
                }
                // Log in to Docker Hub
                sh "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"

                // Push the Docker image to Docker Hub
                sh "docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:${env.BUILD_ID}"
                sh "docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:latest"
            }
        }

        stage('Run Docker Container') {
            steps {
                // Run the Docker container from the built image with port 8000 mapped to the host
                script {
                    sh "docker run -p 8000:80 -d --name my_app ${env.IMAGE_NAME}"
                }
            }
        }
    }
}
