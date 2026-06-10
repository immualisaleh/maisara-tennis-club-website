pipeline {
    agent any

    environment {
        EC2_HOST = '13.49.230.227'
        EC2_USER = 'ec2-user'
        APP_DIR = '/home/ec2-user/maisara-tennis-club-website'
        CONTAINER_NAME = 'maisara-container'
        IMAGE_NAME = 'maisara-website'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub...'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['e1f86260-8928-4488-876b-975ccd78e31f']) {
                    bat """
                    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} "cd ${APP_DIR} && git pull origin main && sudo docker build -t ${IMAGE_NAME} . && sudo docker stop ${CONTAINER_NAME} || true && sudo docker rm ${CONTAINER_NAME} || true && sudo docker run -d -p 80:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment to AWS EC2 completed successfully.'
        }

        failure {
            echo 'Deployment to AWS EC2 failed.'
        }
    }
}