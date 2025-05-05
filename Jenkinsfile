pipeline {
    agent {
        label 'windows'
    }

    environment {
        DOCKER_BUILDKIT = 1
        COMPOSE_DOCKER_CLI_BUILD = 1
    }

    options {
        timeout(time: 20, unit: 'MINUTES')
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/bindu293/Music-system.git'
            }
        }

        stage('Clean Existing Containers') {
            steps {
                bat '''
                docker ps -q -f name=bindu-music | findstr . >nul && docker stop bindu-music && docker rm bindu-music
                docker ps -q -f name=mongo | findstr . >nul && docker stop mongo && docker rm mongo
                docker-compose down || echo docker-compose down failed or not needed
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                echo '📦 Building Docker images...'
                bat 'docker-compose build --no-cache'
            }
        }

        stage('Run Containers') {
            steps {
                echo '🚀 Starting containers...'
                bat 'docker-compose up -d'
            }
        }

        stage('Health Check') {
            steps {
                echo '🔍 Performing health check...'
                bat '''
                ping -n 21 127.0.0.1 > nul
                curl http://localhost:3000 || exit 1
                '''
            }
        }
    }

    post {
        always {
            echo '🧹 Final cleanup and restart containers...'
            bat '''
            docker-compose down
            docker-compose up -d
            '''
        }
    }
}
