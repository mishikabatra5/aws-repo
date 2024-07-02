pipeline {
    agent any

    environment {
        ECR_REGISTRY = '905418473125.dkr.ecr.us-east-1.amazonaws.com/mishika'
        ECR_REPOSITORY = 'mishika'
        IMAGE_TAG = "${env.BUILD_ID}"
        AWS_ROLE_ARN = 'arn:aws:iam::905418473125:role/ecr_docker_image'
        OIDC_PROVIDER_URL = 'https://token.actions.githubusercontent.com'
        OIDC_AUDIENCE = 'https://github.com/mishikabatra5/aws-repo.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/mishikabatra5/aws-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}")
                }
            }
        }

        stage('Assume Role with OIDC') {
            steps {
                withAWS(role: "${AWS_ROLE_ARN}", roleSessionName: 'JenkinsSession', region: 'your-region') {
                    sh 'aws ecr get-login-password --region your-region | docker login --username AWS --password-stdin ${ECR_REGISTRY}'
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    docker.image("${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}").push()
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker rmi ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                }
            }
        }
    }
}
