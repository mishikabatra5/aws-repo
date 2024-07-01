pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'  // Replace with your AWS region
        AWS_ACCOUNT_ID = '905418473125'   // Replace with your AWS account ID
        ECR_REPO = 'mishika'       // Replace with your ECR repository name
        IMAGE_TAG = "latest"              // or any specific tag you want to use
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.withRegistry("https://${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com", 'ecr:us-east-1') {
                        def customImage = docker.build("${ECR_REPO}:${IMAGE_TAG}")
                        customImage.push()
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Failed to build or push Docker image.'
        }
    }
}
