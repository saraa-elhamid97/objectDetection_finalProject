pipeline {
    agent any
    parameters { string(name: 'YOLO_IMAGE_URL', defaultValue: '', description: '') }
    stages {
         stage('kubeconfig '){
            steps{
                sh '''
                    aws eks --region us-east-1 update-kubeconfig --name k8s-main
                    kubectl config set-context --current --namespace=saraa
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    cd k8s
                    # Replace the image field in yolo5.yaml with the provided YOLO_IMAGE_URL
                    sed -i 's#image: .*#image: ${YOLO_IMAGE_URL}#' yolo5.yaml
                    kubectl apply -f yolo5.yaml
                    kubectl apply -f yolo5-autoscaler.yaml
                '''
            }
        }
    }
}