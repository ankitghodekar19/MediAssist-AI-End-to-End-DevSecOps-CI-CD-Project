pipeline {
    agent any

    environment {
        DOCKER_USER = "ankitghodekar"

        BACKEND_IMAGE = "${DOCKER_USER}/health-backend"
        FRONTEND_IMAGE = "${DOCKER_USER}/health-frontend"
        AI_IMAGE = "${DOCKER_USER}/health-ai"

        BUILD_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t $BACKEND_IMAGE:$BUILD_TAG .'
                    sh 'docker tag $BACKEND_IMAGE:$BUILD_TAG $BACKEND_IMAGE:latest'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t $FRONTEND_IMAGE:$BUILD_TAG .'
                    sh 'docker tag $FRONTEND_IMAGE:$BUILD_TAG $FRONTEND_IMAGE:latest'
                }
            }
        }

        stage('Build AI') {
            steps {
                dir('AI Models') {
                    sh 'docker build -t $AI_IMAGE:$BUILD_TAG .'
                    sh 'docker tag $AI_IMAGE:$BUILD_TAG $AI_IMAGE:latest'
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhubcred',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Images') {
            steps {

                sh 'docker push $BACKEND_IMAGE:$BUILD_TAG'
                sh 'docker push $BACKEND_IMAGE:latest'

                sh 'docker push $FRONTEND_IMAGE:$BUILD_TAG'
                sh 'docker push $FRONTEND_IMAGE:latest'

                sh 'docker push $AI_IMAGE:$BUILD_TAG'
                sh 'docker push $AI_IMAGE:latest'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose down'
                sh 'docker compose pull'
                sh 'docker compose up -d'
            }
        }
    

        stage('Cleanup') {
            steps {
                sh 'docker system prune -af'
            }
        }
    }  
}