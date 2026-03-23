pipeline {
    agent any
    environment {
        // Define your image name once to avoid typos
        IMAGE_NAME = "siripo/myapp:v1"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME% ."
            }
        }
stage('Push to Docker Hub') {
    steps {
        // This 'docker-hub-creds' must match the ID you just created in Jenkins
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', 
                         passwordVariable: 'PASS', 
                         usernameVariable: 'USER')]) {
            bat "echo %PASS% | docker login -u %USER% --password-stdin"
            bat "docker push siripo/myapp:v1"
        }
    }
}
    }
}