pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Code checked out from GitHub'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t maisara-website .'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop maisara-container || exit /b 0'
                bat 'docker rm maisara-container || exit /b 0'
            }
        }

        stage('Run New Container') {
            steps {
                bat 'docker run -d -p 8081:80 --name maisara-container maisara-website'
            }
        }
    }

    post {
        success {
            echo 'Maisara website deployed successfully.'
        }

        failure {
            echo 'Deployment failed. Check console output.'
        }
    }
}