pipeline {
    agent any
    parameters { string(name: 'YOLO_IMAGE_URL', defaultValue: '', description: '') }
    stages {
        stage('Deploy') {
            steps {
                sh '''
                    # Replace the image field in yolo5.yaml with the provided YOLO_IMAGE_URL
                    sed -i 's#image: .*#image: ${YOLO_IMAGE_URL}#' yolo5.yaml
                    kubectl apply -f yolo5.yaml
                    kubectl apply -f yolo5-autoscaler.yaml
                '''
            }
        }
    }
}
