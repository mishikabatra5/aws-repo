pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1' // Set your AWS region
        ECR_REPO_NAME = 'mishika' // Set your ECR repository name
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
                        aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin <account-id>.dkr.ecr.$AWS_REGION.amazonaws.com
                    '''
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh '''
                        docker tag my-docker-image:latest <account-id>.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO_NAME:$IMAGE_TAG
                    '''
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    sh '''
                        docker push <account-id>.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO_NAME:$IMAGE_TAG
                    '''
                }
            }
        }
    }
}
