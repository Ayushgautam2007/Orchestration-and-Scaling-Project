pipeline {
agent any

environment {
    AWS_REGION = 'ap-south-1'
    ACCOUNT_ID = '583067667472'
    ECR_REGISTRY = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
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
            pwd
            ls -la
            git --version
            docker --version
            aws --version
            '''
        }
    }

    stage('Build Hello Service') {
        steps {
            dir('backend/helloService') {
                sh '''
                docker build -t hello-service:latest .
                '''
            }
        }
    }

    stage('Build Profile Service') {
        steps {
            dir('backend/profileService') {
                sh '''
                docker build -t profile-service:latest .
                '''
            }
        }
    }

    stage('Build Frontend') {
        steps {
            dir('frontend') {
                sh '''
                docker build -t frontend:latest .
                '''
            }
        }
    }

    stage('Login to ECR') {
        steps {
            sh '''
            aws ecr get-login-password --region ${AWS_REGION} | \
            docker login --username AWS --password-stdin ${ECR_REGISTRY}
            '''
        }
    }

    stage('Tag Images') {
        steps {
            sh '''
            docker tag hello-service:latest ${ECR_REGISTRY}/helloservice:latest

            docker tag profile-service:latest ${ECR_REGISTRY}/profileservice:latest

            docker tag frontend:latest ${ECR_REGISTRY}/frontend:latest
            '''
        }
    }

    stage('Push Images') {
        steps {
            sh '''
            docker push ${ECR_REGISTRY}/helloservice:latest

            docker push ${ECR_REGISTRY}/profileservice:latest

            docker push ${ECR_REGISTRY}/frontend:latest
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

