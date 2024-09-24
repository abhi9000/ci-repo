pipeline {
    agent {
        kubernetes {
            inheritFrom 'kaniko'
            defaultContainer 'kaniko'
        }
    }
    environment {
        AWS_ACCOUNT_ID = '891377024727'
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_REPO = 'my-express-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${ECR_REPO}:${IMAGE_TAG}"
    }
    stages {
        stage('Checkout') {
            steps {
                // The code is checked out to /workspace in the 'jnlp' container
                checkout scm
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                container('kaniko') {
                    sh '''
                    /kaniko/executor \
                      --dockerfile=Dockerfile \
                      --context=/workspace \
                      --destination=${ECR_URI} \
                      --cache=true \
                      --insecure \
                      --skip-tls-verify
                    '''
                }
            }
        }
    }
}

