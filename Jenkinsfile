pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "reverse-proxy-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out code..."
                git url: 'https://github.com/vigneshkavivk/gho.git', branch: 'main'
            }
        }

        stage('Build & Deploy Containers') {
            steps {
                echo "Stopping any existing containers..."
                sh "docker-compose down || true"

                echo "Building and starting Python app + Nginx..."
                sh "docker-compose up --build -d"
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking running containers..."
                sh "docker ps"

                echo "Checking Python app response via Nginx..."
                sh "curl -s http://localhost:80"
            }
        }
    }

    post {
        always {
            echo 'Jenkins pipeline finished.'
        }
        failure {
            echo 'Deployment failed. Check logs.'
        }
    }
}
