pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "reverse-proxy-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/yourusername/yourrepo.git', branch: 'main'
            }
        }

        stage('Build & Deploy') {
            steps {
                script {
                    // Build and start containers
                    sh "docker-compose down"
                    sh "docker-compose up --build -d"
                }
            }
        }

        stage('Verify') {
            steps {
                script {
                    sh "docker ps"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}
