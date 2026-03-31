pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/DishaZalavadiya/nodeapp_demo.git'
            }
        }

        stage('Install Docker (if not present)') {
            steps {
                sh '''
                which docker || (
                    sudo apt update &&
                    sudo apt install -y docker.io
                )
                '''
            }
        }

        stage('Start Docker Service') {
            steps {
                sh '''
                sudo systemctl start docker
                sudo systemctl enable docker
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t nodeapp_demo:latest .'
            }
        }

        stage('Deploy App') {
            steps {
                sh '''
                sudo docker rm -f nodeapp_container || true
                sudo docker run -d -p 3000:3000 --name nodeapp_container nodeapp_demo:latest
                '''
            }
        }

        stage('Verify App') {
            steps {
                sh 'sleep 10 && curl -f http://localhost:3000'
            }
        }
    }

    post {
        success {
            echo '✅ App deployed successfully!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
