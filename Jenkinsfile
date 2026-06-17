pipeline {
agent any

```
environment {
    AWS_REGION = 'ap-south-1'
    ACCOUNT_ID = '076124126275'
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
            dir('backend/helloservice') {
                sh '''
                docker build -t hello-service .
                '''
            }
        }
    }

    stage('Build Profile Service') {
        steps {
            dir('backend/profileservice') {
                sh '''
                docker build -t profile-service .
                '''
            }
        }
    }

    stage('Build Frontend') {
        steps {
            dir('frontend') {
                sh '''
                docker build -t frontend .
                '''
            }
        }
    }

    stage('Login to ECR') {
        steps {
            sh '''
            aws ecr get-login-password --region ${AWS_REGION} | \
            docker login --username AWS \
            --password-stdin ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
            '''
        }
    }

    stage('Tag Images') {
        steps {
            sh '''
            docker tag hello-service:latest \
            ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/helloservice:latest

            docker tag profile-service:latest \
            ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/profilefile:latest

            docker tag frontend:latest \
            ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/mern-frontend:latest
            '''
        }
    }

    stage('Push Images') {
        steps {
            sh '''
            docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/helloservice:latest

            docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/profilefile:latest

            docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/mern-frontend:latest
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
```

}
