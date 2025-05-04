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
        sh 'npm install'
    }
}


        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Run Application') {
            steps {
                // Force remove existing conflicting container (if any)
                sh 'docker rm -f music-streamer || exit 0'
                // Start containers
                sh 'docker-compose up -d'
            }
        }
    }

   
