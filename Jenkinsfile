pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1' // Set your AWS region
        AWS_ACCOUNT_ID = '905418473125' // Replace with your AWS account ID
        ECR_REPO_NAME = 'myapp' // Set your ECR repository name
        IMAGE_TAG = 'latest' // Set the tag you want to use for your image
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-docker-image', '-f Dockerfile .')
                }
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    // Authenticate Docker to the ECR registry
                    sh '''
                        aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    '''
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh '''
                        docker tag my-docker-image:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    sh '''
                        docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                    '''
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
