pipeline {
    agent any
    parameters { string(name: 'POLYBOT_IMAGE_URL', defaultValue: '', description: '') }
    stages {
        stage('Deploy') {
            steps {
                // complete this code to deploy to real k8s cluster
                sh '# kubectl apply -f ....'
            }
        }
    }
}