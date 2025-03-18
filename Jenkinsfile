pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') // Учётные данные Docker Hub
        DOCKER_IMAGE_NAME = 'your-dockerhub-username/your-image-name' // Имя образа в Docker Hub
        DOCKER_IMAGE_TAG = 'latest' // Тег образа
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Сборка Docker-образа
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."

                    // Логин в Docker Hub
                    sh "echo ${DOCKER_HUB_CREDENTIALS_PSW} | docker login -u ${DOCKER_HUB_CREDENTIALS_USR} --password-stdin"

                    // Пуш образа в Docker Hub
                    sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main' // Выполнять только при слиянии в ветку main
            }
            steps {
                script {
                    // Остановка и удаление старого контейнера (если есть)
                    sh "docker stop your-container-name || true"
                    sh "docker rm your-container-name || true"

                    // Запуск нового контейнера из образа
                    sh "docker run -d --name your-container-name -p 8080:80 ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline выполнен успешно!'
        }
        failure {
            echo 'Pipeline завершился с ошибкой.'
        }
    }
}
