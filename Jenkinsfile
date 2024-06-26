pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "revhire:latest"
        KUBECONFIG_CREDENTIALS = "kubeconfig-credentials"
    }
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/vamsimalle1790/revhire.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}")
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: "${env.KUBECONFIG_CREDENTIALS}", variable: 'KUBECONFIG')]) {
                    script {
                        sh 'kubectl apply -f deployment.yaml --kubeconfig=$KUBECONFIG'
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
