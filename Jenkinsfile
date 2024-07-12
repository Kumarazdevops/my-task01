pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'flask_app'
        REGISTRY = 'sravankumar0338'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Kumarazdevops/my-task01.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image("${env.DOCKER_IMAGE}").inside {
                        sh 'python -m unittest discover -s tests'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry("https://${env.REGISTRY}", 'docker-credentials-id') {
                        docker.image("${env.DOCKER_IMAGE}").push('latest')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.image("${env.REGISTRY}/${env.DOCKER_IMAGE}:latest").run('-d -p 5000:5000')
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
