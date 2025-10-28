pipeline {
    agent any

    environment {
        REGISTRY_URL = 'myregistry.example.com'
        REPO_NAME    = 'my-python-app'
        IMAGE_TAG    = "${env.BUILD_NUMBER}"
        DOCKER_IMAGE = "${env.REGISTRY_URL}/${env.REPO_NAME}:${env. IMAGE_TAG}"
        REGISTRY_CREDENTIALS = 'my-docker-creds' // Jenkins credentials ID
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
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push to Registry') {
            steps {
                script {
                    docker.withRegistry("https://${REGISTRY_URL}", REGISTRY_CREDENTIALS) {
                        docker.image(DOCKER_IMAGE).push()
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
