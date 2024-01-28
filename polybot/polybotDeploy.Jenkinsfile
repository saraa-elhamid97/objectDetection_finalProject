pipeline {
    agent any
    parameters {
        string(name: 'POLYBOT_IMAGE_URL', defaultValue: '', description: 'Enter the Polybot image URL')
    }
    stages {
        stage('Deploy') {
            steps {
                sh '''
                    cd k8s
                    # Replace the image field in polybot.yaml with the provided POLYBOT_IMAGE_URL
                    sed -i 's#image: .*#image: ${POLYBOT_IMAGE_URL}#' polybot.yaml
                    kubectl apply -f polybot.yaml
                    kubectl apply -f saraaPolybot-ingress.yaml
                '''
            }
        }
    }
}
