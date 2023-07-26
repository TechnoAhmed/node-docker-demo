pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('ecsfargate')
        AWS_SECRET_ACCESS_KEY = credentials('ecsfargate')
        AWS_REGION = 'us-east-2'
        ECS_CLUSTER_NAME = 'cluster-jenkins'
        ECS_SERVICE_NAME = 'service' // e.g., 'my-ecs-service'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/TechnoAhmed/node-docker-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
      
                        def customImage = docker.build("575279257907.dkr.ecr.us-east-2.amazonaws.com/public_repo", ".":${env.BUILD_NUMBER}")
                        customImage.push()
                    }
                }
            }
        }
        stage('Deploy to ECS') {
            steps {
                script {
                    sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin <575279257907.dkr.ecr.us-east-2.amazonaws.com/public_repo>'
                    sh "ecs-cli compose --file docker-compose.yaml --project-name node-docker-demo service up --cluster-config $ECS_CLUSTER_NAME --ecs-profile default --force-deployment"
                }
            }
        }
    }
}
