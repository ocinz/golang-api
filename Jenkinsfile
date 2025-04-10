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
                sh 'cat .env'
            }
        }
        stage('Docker Compose Build') {
            steps {
                sh 'docker compose --env-file .env build'
            }
        }
        stage('Docker Compose Up') {
            steps {
                sh 'docker compose --env-file .env up -d'
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
