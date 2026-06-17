pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '076124126275'

        HELLO_IMAGE   = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/helloservice"
        PROFILE_IMAGE = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/profileservice"
        FRONTEND_IMAGE = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/frontend"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                sh '''
                git --version
                docker --version
                aws --version
                '''
            }
        }

        stage('Build Hello Service') {
            steps {
                sh '''
                cd /Orchestration/Orchestration-and-Scaling-Project/backend/helloservice/
                docker build -t helloservice .
                '''
            }
        }

        stage('Build Profile Service') {
            steps {
                sh '''
                cd /Orchestration/Orchestration-and-Scaling-Project/backend/profileservice/
                docker build -t profileservice .
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                cd /Orchestration/Orchestration-and-Scaling-Project/frontend/
                docker build -t frontend .
                '''
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                docker tag hello-service:latest $HELLO_IMAGE:latest
                docker tag profile-service:latest $PROFILE_IMAGE:latest
                docker tag frontend:latest $FRONTEND_IMAGE:latest
                '''
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                docker push $HELLO_IMAGE:latest
                docker push $PROFILE_IMAGE:latest
                docker push $FRONTEND_IMAGE:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Jenkins console output.'
        }
    }
}
