pipeline {
    agent any
    
    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.jenkins.yml'
        GIT_REPO = 'https://github.com/W-Bjwa04/devops-assignment-2.git'
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Fetching code from GitHub...'
                    git branch: 'main', url: "${env.GIT_REPO}"
                }
            }
        }
        
        stage('Stop Existing Containers') {
            steps {
                script {
                    echo 'Stopping existing containers...'
                    sh """
                        docker-compose -p part2 -f ${DOCKER_COMPOSE_FILE} down || true
                    """
                }
            }
        }
        
        stage('Build and Deploy with Docker Compose') {
            steps {
                script {
                    echo 'Building and starting containers...'
                    sh """
                        docker-compose -p part2 -f ${DOCKER_COMPOSE_FILE} up -d --build
                    """
                }
            }
        }
        
        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying containers are running...'
                    sh """
                        docker-compose -p part2 -f ${DOCKER_COMPOSE_FILE} ps
                    """
                }
            }
        }
        
        stage('Health Check') {
            steps {
                script {
                    echo 'Waiting for services to be ready...'
                    sleep(time: 30, unit: 'SECONDS')
                    
                    echo 'Checking backend health...'
                    sh """
                        curl -f http://localhost:5001/health || exit 1
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
            echo 'Application is running at:'
            echo 'Frontend: http://localhost:3001'
            echo 'Backend: http://localhost:5001'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
            sh """
                docker-compose -f ${DOCKER_COMPOSE_FILE} logs
            """
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}
