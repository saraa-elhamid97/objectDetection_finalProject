pipeline {
    agent any

    environment {
        NEXUS_CREDENTIALS_ID = 'nexus'
        NEXUS_URL = 'localhost:8083/'
        NEXUS_VERSION = 'nexus3'
        PROTOCOL = 'http'
        REPOSITORY = 'imageprediction'
        VERSION = 'v1.0'
        IMAGE_NAME = 'saraa-polybot'
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} polybot/
                    docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${NEXUS_URL}${REPOSITORY}/${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh '''
                    docker login -u "${NEXUS_USERNAME}" -p "${NEXUS_PASSWORD}" localhost:8083
                    docker push ${NEXUS_URL}${REPOSITORY}/${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Trigger Deploy') {
            steps {
                script {
                    build job: 'PolybotDeploy', wait: false, parameters: [
                        string(name: 'POLYBOT_IMAGE_URL', value: "${NEXUS_URL}${REPOSITORY}/${IMAGE_NAME}:${BUILD_NUMBER}")
                    ]
                }
            }
        }
    }
}
