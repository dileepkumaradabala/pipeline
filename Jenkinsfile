pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your-dockerhub-username/flask-app:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/dileepkumaradabala/pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Clean Up') {
            steps {
                sh "docker rmi $DOCKER_IMAGE"
            }
        }
    }
}
