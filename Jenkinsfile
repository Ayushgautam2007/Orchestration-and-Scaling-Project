pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ACCOUNT_ID = '583067667472'
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
                cd /Orchestration-and-Scaling-Project/backend/helloservice/
                docker build -t hello-service .
                '''
            }
        }

        stage('Build Profile Service') {
            steps {
                sh '''
                cd /Orchestration-and-Scaling-Project/backend/profileservice/
                docker build -t profile-service .
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                cd /Orchestration-and-Scaling-Project/frontend/
                docker build -t frontend .
                '''
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 583067667472.dkr.ecr.ap-south-1.amazonaws.com
                '''
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                docker tag hello-service:latest 583067667472.dkr.ecr.ap-south-1.amazonaws.com/helloservice:latest

                docker tag profile-service:latest 583067667472.dkr.ecr.ap-south-1.amazonaws.com/profileservice:latest

                docker tag frontend:latest 583067667472.dkr.ecr.ap-south-1.amazonaws.com/frontend:latest
                '''
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                docker push 583067667472.dkr.ecr.ap-south-1.amazonaws.com/helloservice:latest

                docker push 583067667472.dkr.ecr.ap-south-1.amazonaws.com/profileservice:latest

                docker push 583067667472.dkr.ecr.ap-south-1.amazonaws.com/frontend:latest
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
