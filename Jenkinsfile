pipeline {
    agent any

    // Windows fix: Jenkins runs as a Windows service, so 'sh' steps are replaced with 'bat',
    // '|| true' with '|| exit 0', and Docker Desktop's per-user CLI is added to PATH.
    environment {
        PATH = "C:\\Users\\Soham\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;${env.PATH}"
        DOCKER_CONFIG = "C:\\Users\\Soham\\.docker"
    }

    stages {
        stage('Checkout Code') {
            steps { checkout scm }
        }

        stage('Install Dependencies') {
            steps { bat 'pip install -r requirements.txt' }
        }

        stage('Test Application') {
            steps { bat 'python -m py_compile app.py' }
        }

        stage('Build Docker Image') {
            steps { bat 'docker build -t student-flask-app .' }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop student-app || exit 0'
                bat 'docker rm student-app || exit 0'
            }
        }

        stage('Deploy Application') {
            steps {
                bat 'docker run -d -p 5000:5000 --name student-app student-flask-app'
            }
        }

        stage('Verify Deployment') {
            steps { bat 'docker ps' }
        }
    }

    post {
        success {
            echo 'Application deployed successfully using Jenkins.'
        }
        failure {
            echo 'Application deployment failed.'
        }
    }
}
