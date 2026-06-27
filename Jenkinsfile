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

        stage('SonarQube Analysis') {
            steps {
                script {

                    def scannerHome = tool 'sonar'

                    withSonarQubeEnv('sonar') {

                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=MediAssist-AI \
                        -Dsonar.projectName=MediAssist-AI \
                        -Dsonar.sources=.
                        """

                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {

                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }

            }
        }

        stage('OWASP Dependency Check') {
            steps {

                dependencyCheck(
                    odcInstallation: 'dc',
                    additionalArguments: '--scan .'
                )

            }
        }

        stage('Publish OWASP Report') {
            steps {

                dependencyCheckPublisher(
                    pattern: '**/dependency-check-report.xml'
                )

                publishHTML(target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: '.',
                        reportFiles: 'dependency-check-report.html',
                        reportName: 'OWASP Dependency Check Report'
                ])

            }
        }

        stage('Trivy Filesystem Scan') {
            steps {

                sh '''
                trivy fs . \
                --severity HIGH,CRITICAL \
                --format table \
                --output trivy-fs-report.txt
                '''

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

        stage('Trivy Image Scan') {
            steps {

                sh '''
                trivy image \
                --severity HIGH,CRITICAL \
                --format table \
                --output backend-report.txt \
                $BACKEND_IMAGE:$BUILD_TAG
                '''

                sh '''
                trivy image \
                --severity HIGH,CRITICAL \
                --format table \
                --output frontend-report.txt \
                $FRONTEND_IMAGE:$BUILD_TAG
                '''

                sh '''
                trivy image \
                --severity HIGH,CRITICAL \
                --format table \
                --output ai-report.txt \
                $AI_IMAGE:$BUILD_TAG
                '''

            }
        }

        stage('Publish Trivy Reports') {
            steps {

                archiveArtifacts artifacts: '*.txt', fingerprint: true

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

        stage('Health Check') {
            steps {

                sh '''
                sleep 20

                curl -f http://localhost:3000
                curl -f http://localhost:5000
                curl -f http://localhost:8000
                '''

            }
        }

        stage('Cleanup') {
            steps {

                sh 'docker system prune -af'

            }
        }

    }

    post {

        success {

            emailext(
                subject: "Jenkins Build SUCCESS #${BUILD_NUMBER}",
                body: "Build ${BUILD_NUMBER} completed successfully.",
                to: "yourmail@gmail.com"
            )

        }

        failure {

            emailext(
                subject: "Jenkins Build FAILED #${BUILD_NUMBER}",
                body: "Build ${BUILD_NUMBER} failed. Please check Jenkins.",
                to: "yourmail@gmail.com"
            )

        }

        always {

            archiveArtifacts(
                artifacts: '**/*.html,**/*.xml,**/*.txt',
                allowEmptyArchive: true,
                fingerprint: true
            )

            cleanWs()

        }

    }

}





















// pipeline {
//     agent any

//     environment {

//         DOCKER_USER = "ankitghodekar"

//         BACKEND_IMAGE = "${DOCKER_USER}/health-backend"
//         FRONTEND_IMAGE = "${DOCKER_USER}/health-frontend"
//         AI_IMAGE = "${DOCKER_USER}/health-ai"

//         BUILD_TAG = "${BUILD_NUMBER}"
//     }

//     stages {

//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }
//         stage('SonarQube Analysis') {
//             steps {
//                 script {
//                     def scannerHome = tool 'sonar'

//                     withSonarQubeEnv('sonar') {

//                         sh """
//                         ${scannerHome}/bin/sonar-scanner \
//                         -Dsonar.projectKey=MediAssist-AI \
//                         -Dsonar.projectName=MediAssist-AI \
//                         -Dsonar.sources=.
//                         """
//                     }
//                 }
//             }
//         }

//         stage('Quality Gate') {
//             steps {
//                 timeout(time: 5, unit: 'MINUTES') {
//                     waitForQualityGate abortPipeline: true
//                 }
//             }
//         }

//         stage('OWASP Dependency Check') {
//             steps {

//                 dependencyCheck(
//                         odcInstallation: 'dc',
//                         additionalArguments: '--scan .'
//                 )

//                 dependencyCheckPublisher(
//                         pattern: '**/dependency-check-report.xml'
//                 )
//             }
//         }

//         stage('Trivy Filesystem Scan') {
//             steps {
//                 sh '''
//                 trivy fs \
//                 --severity HIGH,CRITICAL \
//                 --format table \
//                 .
//                 '''
//             }
//         }

//         stage('Build Backend') {
//             steps {
//                 dir('backend') {

//                     sh 'docker build -t $BACKEND_IMAGE:$BUILD_TAG .'
//                     sh 'docker tag $BACKEND_IMAGE:$BUILD_TAG $BACKEND_IMAGE:latest'

//                 }
//             }
//         }

//         stage('Build Frontend') {
//             steps {
//                 dir('frontend') {

//                     sh 'docker build -t $FRONTEND_IMAGE:$BUILD_TAG .'
//                     sh 'docker tag $FRONTEND_IMAGE:$BUILD_TAG $FRONTEND_IMAGE:latest'

//                 }
//             }
//         }

//         stage('Build AI') {
//             steps {
//                 dir('AI Models') {

//                     sh 'docker build -t $AI_IMAGE:$BUILD_TAG .'
//                     sh 'docker tag $AI_IMAGE:$BUILD_TAG $AI_IMAGE:latest'

//                 }
//             }
//         }

//         stage('Trivy Image Scan') {
//             steps {

//                 sh '''
//                 trivy image --severity HIGH,CRITICAL $BACKEND_IMAGE:$BUILD_TAG
//                 '''

//                 sh '''
//                 trivy image --severity HIGH,CRITICAL $FRONTEND_IMAGE:$BUILD_TAG
//                 '''

//                 sh '''
//                 trivy image --severity HIGH,CRITICAL $AI_IMAGE:$BUILD_TAG
//                 '''
//             }
//         }

//         stage('Docker Login') {
//             steps {

//                 withCredentials([
//                     usernamePassword(
//                         credentialsId: 'dockerhubcred',
//                         usernameVariable: 'DOCKER_USER',
//                         passwordVariable: 'DOCKER_PASS'
//                     )
//                 ]) {

//                     sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'

//                 }
//             }
//         }

//         stage('Push Images') {
//             steps {

//                 sh 'docker push $BACKEND_IMAGE:$BUILD_TAG'
//                 sh 'docker push $BACKEND_IMAGE:latest'

//                 sh 'docker push $FRONTEND_IMAGE:$BUILD_TAG'
//                 sh 'docker push $FRONTEND_IMAGE:latest'

//                 sh 'docker push $AI_IMAGE:$BUILD_TAG'
//                 sh 'docker push $AI_IMAGE:latest'
//             }
//         }

//         stage('Deploy') {
//             steps {

//                 sh 'docker compose down'

//                 sh 'docker compose pull'

//                 sh 'docker compose up -d'
//             }
//         }

//         stage('Cleanup') {
//             steps {

//                 sh 'docker system prune -af'

//             }
//         }
//     }

//     post {

//         success {
//             echo "Pipeline Completed Successfully."
//         }

//         failure {
//             echo "Pipeline Failed."
//         }

//         always {

//             archiveArtifacts artifacts: '**/dependency-check-report.xml', allowEmptyArchive: true

//             archiveArtifacts artifacts: '**/trivy*.txt', allowEmptyArchive: true
//         }
//     }
// }













// // pipeline {
// //     agent any

// //     environment {
// //         DOCKER_USER = "ankitghodekar"

// //         BACKEND_IMAGE = "${DOCKER_USER}/health-backend"
// //         FRONTEND_IMAGE = "${DOCKER_USER}/health-frontend"
// //         AI_IMAGE = "${DOCKER_USER}/health-ai"

// //         BUILD_TAG = "${BUILD_NUMBER}"
// //     }

// //     stages {

// //         stage('Checkout') {
// //             steps {
// //                 checkout scm
// //             }
// //         }

// //         stage('Build Backend') {
// //             steps {
// //                 dir('backend') {
// //                     sh 'docker build -t $BACKEND_IMAGE:$BUILD_TAG .'
// //                     sh 'docker tag $BACKEND_IMAGE:$BUILD_TAG $BACKEND_IMAGE:latest'
// //                 }
// //             }
// //         }

// //         stage('Build Frontend') {
// //             steps {
// //                 dir('frontend') {
// //                     sh 'docker build -t $FRONTEND_IMAGE:$BUILD_TAG .'
// //                     sh 'docker tag $FRONTEND_IMAGE:$BUILD_TAG $FRONTEND_IMAGE:latest'
// //                 }
// //             }
// //         }

// //         stage('Build AI') {
// //             steps {
// //                 dir('AI Models') {
// //                     sh 'docker build -t $AI_IMAGE:$BUILD_TAG .'
// //                     sh 'docker tag $AI_IMAGE:$BUILD_TAG $AI_IMAGE:latest'
// //                 }
// //             }
// //         }

// //         stage('Docker Login') {
// //             steps {
// //                 withCredentials([
// //                     usernamePassword(
// //                         credentialsId: 'dockerhubcred',
// //                         usernameVariable: 'DOCKER_USER',
// //                         passwordVariable: 'DOCKER_PASS'
// //                     )
// //                 ]) {
// //                     sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
// //                 }
// //             }
// //         }

// //         stage('Push Images') {
// //             steps {
// //                 sh 'docker push $BACKEND_IMAGE:$BUILD_TAG'
// //                 sh 'docker push $BACKEND_IMAGE:latest'

// //                 sh 'docker push $FRONTEND_IMAGE:$BUILD_TAG'
// //                 sh 'docker push $FRONTEND_IMAGE:latest'

// //                 sh 'docker push $AI_IMAGE:$BUILD_TAG'
// //                 sh 'docker push $AI_IMAGE:latest'
// //             }
// //         }

// //         stage('Deploy') {
// //             steps {
// //                 sh 'docker compose down'
// //                 sh 'docker compose pull'
// //                 sh 'docker compose up -d'
// //             }
// //         }

// //         stage('Cleanup') {
// //             steps {
// //                 sh 'docker system prune -af'
// //             }
// //         }
// //     }
// // }