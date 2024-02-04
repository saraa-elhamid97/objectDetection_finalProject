pipeline {
    agent any
    environment{
       ECR_URL = '933060838752.dkr.ecr.us-east-2.amazonaws.com'
       IMAGE_NAME = 'saraa-polybot'
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin $ECR_URL
                docker build -t polybot:$BUILD_NUMBER polybot/
                docker tag polybot:$BUILD_NUMBER $ECR_URL/$IMAGE_NAME:$BUILD_NUMBER
                docker push $ECR_URL/$IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
        stage('Trigger Deploy') {
            steps {
                build job: 'PolybotDeploy', wait: false, parameters: [
                    string(name: 'POLYBOT_IMAGE_URL', value: "$ECR_URL/$IMAGE_NAME:$BUILD_NUMBER")
        ]
    }
}

    }
}