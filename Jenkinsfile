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

        stage('Install Docker Compose') {
            steps {
                echo "Installing Docker Compose inside Jenkins container..."
                sh '''
                    if ! command -v docker-compose >/dev/null 2>&1; then
                        curl -SL https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
                        chmod +x /usr/local/bin/docker-compose
                    fi
                    docker-compose version
                '''
            }
        }

        stage('Build & Deploy Containers') {
            steps {
                echo "Stopping any existing containers..."
                sh "docker compose down || true"
        
                echo "Building and starting Python app + Nginx..."
                sh "docker compose up --build -d"
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
