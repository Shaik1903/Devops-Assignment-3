pipeline {
    agent { label 'windows' }

    environment {
        APP_NAME = 'flask-app'
        DOCKER_IMAGE = 'flask-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Shaik1903/Devops-Assignment-3.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        sh "docker run -d --name ${APP_NAME} -p 5000:5000 ${DOCKER_IMAGE}"
                        sleep time: 10, unit: 'SECONDS'
                        sh 'curl -f http://localhost:5000'
                        echo 'Tests passed!'
                    } catch (Exception e) {
                        error 'Tests failed!'
                    } finally {
                        sh "docker stop ${APP_NAME} || exit 0"
                        sh "docker rm ${APP_NAME} || exit 0"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh """
                        echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin
                        docker tag ${DOCKER_IMAGE} your-dockerhub-username/${DOCKER_IMAGE}
                        docker push your-dockerhub-username/${DOCKER_IMAGE}
                        """
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker rmi ${DOCKER_IMAGE} || exit 0"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
