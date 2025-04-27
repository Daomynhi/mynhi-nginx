pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'mynhi-nginx'
        DOCKER_REGISTRY = 'docker.io/daomynhi'
        BRANCH = 'main'
        IMAGE_TAG = "${GIT_COMMIT}"
    }
    stages {
        stage('Checkout') {
            steps {
                // Lấy mã nguồn từ Git repository
                git branch: "${BRANCH}", url: 'https://github.com/Daomynhi/mynhi-nginx.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Xây dựng Docker image từ Dockerfile
                script {
                    dockerImage = docker.build("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                // Đẩy Docker image lên Docker Registry
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
    post {
        always {
            // Dọn dẹp workspace nếu cần
            cleanWs()
        }
    }
}

