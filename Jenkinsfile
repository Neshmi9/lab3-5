pipeline {
    agent any

    environment {
        GITHUB_URL = 'https://github.com/Neshmi9/lab3-5.git'
        DOCKER_IMAGE_DEV = 'cithit/alhayen-dev'
        DOCKER_IMAGE_PROD = 'cithit/alhayen-prod'
        IMAGE_TAG = "build-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: "${GITHUB_URL}"]]])
            }
        }

        stage('Lint HTML') {
            steps {
                sh 'npm install htmlhint --save-dev'
                sh 'npx htmlhint *.html'
            }
        }

        stage('Build Dev Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_DEV}:${IMAGE_TAG}", "-f Dockerfile.dev .")
                }
            }
        }

        stage('Build Prod Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_PROD}:${IMAGE_TAG}", "-f Dockerfile.prod .")
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                echo 'Skipping Docker image push for this lab'
            }
        }

        stage('Serve Webpage') {
            steps {
                echo 'Use Docker run to view the resulting page in the browser'
            }
        }
    }

    post {
        success {
            echo "Build completed successfully"
        }
        failure {
            echo "Build failed"
        }
    }
}
