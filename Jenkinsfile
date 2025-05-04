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
                    sh 'npm install'
                }
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

    post {
        failure {
            emailext(
                subject: "Jenkins Build Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "Check the Jenkins build logs here: ${env.BUILD_URL}",
                to: "gorlebindusree@gmail.com"
            )
        }
        success {
            emailext(
                subject: "Jenkins Build Succeeded: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "Your build completed successfully.\nCheck: ${env.BUILD_URL}",
                to: "gorlebindusree@gmail.com"
            )
        }
    }
}
