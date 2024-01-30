pipeline {
    agent any
    parameters {
        string(name: 'POLYBOT_IMAGE_URL', defaultValue: '', description: 'Enter the Polybot image URL')
    }
    stages {
        stage('kubeconfig ') {
            steps {
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
                    pwd
                    echo "Before modification:"
                    cat polybot.yaml  # Print the content before modification
                    sed -i "s#image: .*#image: ${POLYBOT_IMAGE_URL}#" polybot.yaml
                    echo "After modification:"
                    cat polybot.yaml  # Print the content after modification
                    kubectl apply -f polybot.yaml
                    kubectl apply -f saraaPolybot-ingress.yaml
                '''
            }
        }
    }
}
