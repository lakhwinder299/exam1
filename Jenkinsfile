#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/lakhwinder299/exam1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("my_web_app_image:${env.BUILD_ID}", "-f Dockerfile .")
                    env.IMAGE_NAME = "my_web_app_image:${env.BUILD_ID}"
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            environment {
                DOCKER_HUB_USERNAME = 'lakhwinder299'
                DOCKER_HUB_REPOSITORY = 'my_web_app_image'
                DOCKER_HUB_PASSWORD = 'dockerhub@123'
            }
            steps {
                script {
                    sh "docker tag ${env.IMAGE_NAME} ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:${env.BUILD_ID}"
                    sh "docker tag ${env.IMAGE_NAME} ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:latest"
                }
                sh "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
                sh "docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:${env.BUILD_ID}"
                sh "docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:latest"
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker container stop my_web_app_container"
                    sh "docker container rm my_web_app_container"
                    sh "docker run -p 8000:80 -d --name my_web_app_container ${env.IMAGE_NAME}"
                }
            }
        }
    }
}
