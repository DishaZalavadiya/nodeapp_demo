
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/DishaZalavadiya/nodeapp_demo.git'
            }
        }
        stage('install Docker') {
            steps {
                sh 'sudo apt install docker.io -y'
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
                sh 'docker build -t nodeapp_demo:latest .'
            }
        }
        
        stage('Add jenkins to Docker Group') {
            steps {
                sh '''
                sudo usermod -aG docker jenkins
                '''
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d --build'
            }
        }

        stage('Verify App') {
            steps {
                sh 'sleep 10 && curl -f http://localhost:3000 || exit 1'
            }
        }
    }
}
