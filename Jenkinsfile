pipeline {
    agent any
    
    environment {
        SECRET_VAR = credentials('secret_text')
        DOCKER_PORT = '8081'  // Changed port
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
                script {
                    try {
                        sh '''
                            docker stop custom-nginx || true
                            docker rm custom-nginx || true
                            docker run -d -p ${DOCKER_PORT}:80 --name custom-nginx custom-nginx
                        '''
                    } catch (Exception e) {
                        sh '''
                            echo "Attempting cleanup..."
                            docker stop custom-nginx || true
                            docker rm custom-nginx || true
                            docker run -d -p ${DOCKER_PORT}:80 --name custom-nginx custom-nginx
                        '''
                    }
                }
            }
        }
    }
    
    post {
        failure {
            sh '''
                echo "Pipeline failed! Cleaning up..."
                docker stop custom-nginx || true
                docker rm custom-nginx || true
            '''
        }
    }
}
