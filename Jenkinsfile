pipeline {
    agent any

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
                script {
                    if (isUnix()) {
                        sh '''
                        docker ps -q -f name=bindu-music | xargs -r docker rm -f
                        docker ps -q -f name=mongo | xargs -r docker rm -f
                        docker-compose down || echo "docker-compose down failed or not needed"
                        '''
                    } else {
                        bat '''
                        docker ps -q -f name=bindu-music | findstr . >nul && docker stop bindu-music && docker rm bindu-music
                        docker ps -q -f name=mongo | findstr . >nul && docker stop mongo && docker rm mongo
                        docker-compose down || echo docker-compose down failed or not needed
                        '''
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                echo '📦 Building Docker images...'
                script {
                    if (isUnix()) {
                        sh 'docker-compose build --no-cache'
                    } else {
                        bat 'docker-compose build --no-cache'
                    }
                }
            }
        }

        stage('Run Containers') {
            steps {
                echo '🚀 Starting containers...'
                script {
                    if (isUnix()) {
                        sh 'docker-compose up -d'
                    } else {
                        bat 'docker-compose up -d'
                    }
                }
            }
        }

        stage('Health Check') {
            steps {
                echo '🔍 Performing health check...'
                script {
                    if (isUnix()) {
                        sh '''
                        sleep 20
                        curl http://localhost:3000 || exit 1
                        '''
                    } else {
                        bat '''
                        ping -n 21 127.0.0.1 > nul
                        curl http://localhost:3000 || exit 1
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo '🧹 Final cleanup and restart containers...'
            script {
                if (isUnix()) {
                    sh '''
                    docker-compose down
                    docker-compose up -d
                    '''
                } else {
                    bat '''
                    docker-compose down
                    docker-compose up -d
                    '''
                }
            }
        }
    }
}
