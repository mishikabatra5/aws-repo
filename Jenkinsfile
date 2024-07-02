pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_REPO = 'mishika'
        URL_REGISTRY = '905418473125.dkr.ecr.us-east-1.amazonaws.com' // Replace with your ECR registry URL
    }

    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Authenticate using IAM role attached to the instance
                    sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${URL_REGISTRY}"

                    // Build Docker image
                    sh "docker build -t $ECR_REPO ."

                    // Tag Docker image
                    sh "docker tag $ECR_REPO:latest ${URL_REGISTRY}/$ECR_REPO:latest"

                    // Push Docker image to ECR
                    sh "docker push ${URL_REGISTRY}/$ECR_REPO:latest"
                }
            }
        }
    }
}

