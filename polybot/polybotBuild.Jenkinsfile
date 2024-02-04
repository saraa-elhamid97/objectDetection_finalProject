pipeline {
    agent any

    environment {
        NEXUS_URL = 'http://localhost:8083/repository/imagePrediction/'
        IMAGE_NAME = 'saraa-polybot'
    }

    parameters {
        string(name: 'POLYBOT_IMAGE_URL', defaultValue: '', description: 'Enter the Polybot image URL')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh '''
                        docker build -t polybot:$BUILD_NUMBER polybot/
                        docker tag polybot:$BUILD_NUMBER ${NEXUS_URL}${IMAGE_NAME}:$BUILD_NUMBER
                        docker login -u $NEXUS_USERNAME -p $NEXUS_PASSWORD $NEXUS_URL
                        docker push ${NEXUS_URL}${IMAGE_NAME}:$BUILD_NUMBER
                    '''
                }
            }
        }

        stage('Trigger Deploy') {
            steps {
                build job: 'PolybotDeploy', wait: false, parameters: [
                    string(name: 'POLYBOT_IMAGE_URL', value: "${NEXUS_URL}${IMAGE_NAME}:$BUILD_NUMBER")
                ]
            }
        }
    }
}
