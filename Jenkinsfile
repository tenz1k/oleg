pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-go-app'
        CONTAINER_NAME = 'go-app-container'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/tenz1k/oleg.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Сборка Docker-образа локально
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Stop Previous Container') {
            steps {
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 8080:8080 --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }
}
