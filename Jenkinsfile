pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-pat'  // Jenkins에 등록한 Credential ID
        IMAGE_NAME = "jimmy971/guestbook-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AhnMyeonggyun/guestbook.git'
            }
        }
        stage('Build Jar') {
            agent {
                docker { image 'maven:3.8.4-openjdk-11-slim' }
            }
            steps {
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}


