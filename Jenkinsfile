pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "siripo/myapp"
        DOCKER_HUB_CREDS = 'dockerhub-creds'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Siripo591/docker_demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Use double quotes to allow Groovy to inject the variable
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "${DOCKER_HUB_CREDS}",
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        // Use printf or similar for better security on Windows
                        bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push %DOCKER_IMAGE%:latest"
            }
        }
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}