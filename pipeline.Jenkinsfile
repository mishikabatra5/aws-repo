pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '905418473125'
        ECR_REPO = 'mishika'
        IMAGE_TAG = "latest" // or any specific tag you want to use
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
                    docker.withRegistry('https://$(AWS_ACCOUNT_ID).dkr.ecr.$(AWS_DEFAULT_REGION).amazonaws.com', 'ecr:region') {
                        def customImage = docker.build("$(ECR_REPO):$(IMAGE_TAG)")
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
