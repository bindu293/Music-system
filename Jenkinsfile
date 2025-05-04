pipeline {
    agent any

    environment {
        DOCKER_BUILDKIT = 1
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/bindu293/Music-system.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('app') {
                    bat 'npm install'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                bat 'docker-compose build'
            }
        }

        stage('Run Application') {
            steps {
                // Force remove existing conflicting container (if any)
                bat 'docker rm -f music-streamer || exit 0'
                // Start containers
                bat 'docker-compose up -d'
            }
        }
    }

   
}
