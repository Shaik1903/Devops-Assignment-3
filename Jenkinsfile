pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Shaik1903/Devops-Assignment-3.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t simple-calculator-app .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    docker run -d -p 5000:5000 --name calc-app simple-calculator-app
                    sleep 5  # wait for container to start
                    curl -X POST -d "num1=5&num2=3&operation=add" http://localhost:5000/calculate
                    '''
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh 'docker stop calc-app'
                    sh 'docker rm calc-app'
                }
            }
        }
    }

    post {
        always {
            deleteDir()
            sh 'docker system prune -f'
        }
    }
}
