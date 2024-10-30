pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                // Specify the branch you want to clone
                git branch: 'main', url: 'https://github.com/Shaik1903/Devops-Assignment-3.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t flask-app .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Stop any running instance
                    sh 'docker stop flask-app || true && docker rm flask-app || true'
                    // Run the app on port 5000
                    sh 'docker run -d -p 5000:5000 --name flask-app flask-app'
                }
            }
        }
    }
}
