pipeline {
    agent any
    environment {
        DOCKER_ENV = credentials('golang-api-env')
    }
    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }
        stage('Setup .env File') {
            steps {
                script {
                    sh 'cat "$DOCKER_ENV" > .env'
                    echo ".env file created from Jenkins credential"
                }
            }
        }
        stage('Clean Docker') {
            steps {
                script {
                    echo "Membersihkan lingkungan Docker..."
                    sh 'sudo docker compose down || echo "Tidak ada container yang dihentikan"'
                    sh 'sudo docker system prune -af'
                }
            }
        }
        stage('Docker Compose Build') {
            steps {
                sh 'sudo docker compose build'
            }
        }
        stage('Docker Compose Up') {
            steps {
                sh 'sudo docker compose up -d'
            }
        }
    }
    post {
        always {
            sh 'rm -f .env'
        }
        failure {
            echo '❌ Deployment failed.'
        }
        success {
            echo '✅ Deployment successful!'
        }
    }
}
