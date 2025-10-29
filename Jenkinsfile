pipeline {
    agent any

    environment {
        DOCKER_HUB_USER        = 'komalgt' // Your Docker Hub username
        REGISTRY_URL           = 'index.docker.io/v1'
        REPO_NAME              = 'my-python-app'
        IMAGE_TAG              = "${BUILD_NUMBER}"
        DOCKER_IMAGE           = "${DOCKER_HUB_USER}/${REPO_NAME}:${IMAGE_TAG}"
        REGISTRY_CREDENTIALS   = 'my-docker-creds' // Jenkins credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry("https://${REGISTRY_URL}", REGISTRY_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
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
