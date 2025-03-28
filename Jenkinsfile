pipeline {
    agent any
    
    environment {
        SECRET_VAR = credentials('secret_text')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Print Secret') {
            steps {
                sh 'echo "The secret value is: $SECRET_VAR"'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t custom-nginx .'
            }
        }
        
        stage('Run Container') {
            steps {
                sh '''
                    docker stop custom-nginx || true
                    docker rm custom-nginx || true
                    docker run -d -p 8080:80 --name custom-nginx custom-nginx
                '''
            }
        }
    }
}
