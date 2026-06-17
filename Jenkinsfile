pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ACCOUNT_ID = '076124126275'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/yourrepo/project.git'
            }
        }

        stage('Build Hello Service') {
            steps {
                sh '''
                docker build -t helloservice ./HelloService
                '''
            }
        }

        stage('Build Profile Service') {
            steps {
                sh '''
                docker build -t profileservice ./ProfileService
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                docker build -t frontend ./Frontend
                '''
            }
        }

        stage('Login ECR') {
            steps {
                sh '''
                aws ecr get-login-password \
                --region $AWS_REGION | docker login \
                --username AWS \
                --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                docker tag helloservice:latest \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/helloservice:latest

                docker tag profileservice:latest \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/profilefile:latest

                docker tag frontend:latest \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/mern-frontend:latest

                docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/helloservice:latest

                docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/profilefile:latest

                docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/mern-frontend:latest
                '''
            }
        }
    }
}
